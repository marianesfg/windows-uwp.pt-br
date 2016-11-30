---
author: TylerMSFT
title: "Converter um serviço de aplicativo para ser executado no mesmo processo de seu aplicativo host"
description: "Converta o código de serviço de aplicativo executado em um processo separado em segundo plano em código que é executado no mesmo processo de seu provedor de serviços de aplicativo."
translationtype: Human Translation
ms.sourcegitcommit: 7d1c160f8b725cd848bf8357325c6ca284b632ae
ms.openlocfilehash: 80402d12a51ea970f5927dc50cae587be8809b87

---

# Converter um serviço de aplicativo para ser executado no mesmo processo de seu aplicativo host

Um [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceconnection.aspx) permite que outro aplicativo ative seu aplicativo em segundo plano e inicie uma linha direta de comunicação com ele.

Com a introdução dos serviços de aplicativo no processo, dois aplicativos em primeiro plano em execução podem ter uma linha direta de comunicação por meio de uma conexão de serviço de aplicativo. Serviços de aplicativo agora podem ser executados no mesmo processo que o aplicativo em primeiro plano, o que torna a comunicação entre aplicativos muito mais fácil e elimina a necessidade de separar o código de serviço em um projeto separado.

A transformação de um serviço de aplicativo de modelo fora do processo em um modelo no processo exige duas alterações. A primeira é uma alteração de manifesto.

> ```xml
>  <uap:Extension Category="windows.appService">
>          <uap:AppService Name="InProcessAppService" />
>  </uap:Extension>
> ```

Remover o atributo `EntryPoint`. Agora o retorno de chamada [OnBackgroundActivated()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) será usado como o método de retorno de chamada quando o serviço de aplicativo for invocado.

A segunda alteração é mover a lógica de serviço do seu projeto de tarefa em segundo plano separado para métodos que podem ser chamados de **OnBackgroundActivated()**.

Agora seu aplicativo pode executar diretamente o serviço de aplicativo.  Por exemplo:

> ``` cs
> private AppServiceConnection appServiceconnection;
> private BackgroundTaskDeferral appServiceDeferral;
> protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
> {
>     base.OnBackgroundActivated(args);
>     IBackgroundTaskInstance taskInstance = args.TaskInstance;
>     AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
>     appServiceDeferral = taskInstance.GetDeferral();
>     taskInstance.Canceled += OnAppServicesCanceled;
>     appServiceConnection = appService.AppServiceConnection;
>     appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
>     appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
> }
>
> private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
> {
>     AppServiceDeferral messageDeferral = args.GetDeferral();
>     ValueSet message = args.Request.Message;
>     string text = message["Request"] as string;
>              
>     if ("Value" == text)
>     {
>         ValueSet returnMessage = new ValueSet();
>         returnMessage.Add("Response", "True");
>         await args.Request.SendResponseAsync(returnMessage);
>     }
>     messageDeferral.Complete();
> }
>
> private void OnAppServicesCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
> {
>     appServiceDeferral.Complete();
> }
>
> private void AppServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
> {
>     appServiceDeferral.Complete();
> }
> ```

O código acima do método `OnBackgroundActivated` trata a ativação do serviço de aplicativo. Todos os eventos necessários para a comunicação por meio de um [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceconnection.aspx) são registrados, e o objeto de adiamento da tarefa é armazenado para que ele possa ser marcado como concluído quando a comunicação entre os aplicativos terminar.

Quando o aplicativo recebe uma solicitação e lê o [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx) fornecido para ver se as cadeias de caracteres `Key` e `Value` estão presentes. Se eles estiverem presentes, o serviço de aplicativo retornará um par de valores de cadeias `Response` e `True` para o aplicativo do outro lado do **AppServiceConnection**.

Saiba mais sobre como conectar e se comunicar com outros aplicativos em [Criar e consumir um serviço de aplicativo](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service?f=255&MSPPError=-2147217396).



<!--HONumber=Nov16_HO1-->


