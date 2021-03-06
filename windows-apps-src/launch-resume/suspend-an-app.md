---
title: Tratar a suspensão do aplicativo
description: Saiba como salvar dados de aplicativo importantes quando o sistema suspende o seu aplicativo.
ms.assetid: F84F1512-24B9-45EC-BF23-A09E0AC985B0
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: f912e6212346a4019d8421c542a81eb2318dc5d9
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260406"
---
# <a name="handle-app-suspend"></a>Tratar a suspensão do aplicativo

**APIs importantes**

- [**Suspensão**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.suspending)

Saiba como salvar dados de aplicativo importantes quando o sistema suspende o seu aplicativo. O exemplo registra um manipulador de eventos para o evento [**Suspending**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.suspending) e salva uma cadeia de caracteres em um arquivo.

## <a name="register-the-suspending-event-handler"></a>Registrar o manipulador de eventos de suspensão

Registre-se para manipular o evento [**Suspending**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.suspending), que indica que o aplicativo deve salvar seus dados antes de ser suspenso pelo sistema.

```csharp
using System;
using Windows.ApplicationModel;
using Windows.ApplicationModel.Activation;
using Windows.UI.Xaml;

partial class MainPage
{
   public MainPage()
   {
      InitializeComponent();
      Application.Current.Suspending += new SuspendingEventHandler(App_Suspending);
   }
}
```

```vb
Public NotInheritable Class MainPage

   Public Sub New()
      InitializeComponent()
      AddHandler Application.Current.Suspending, AddressOf App_Suspending
   End Sub
   
End Class
```

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();
    Windows::UI::Xaml::Application::Current().Suspending({ this, &MainPage::App_Suspending });
}
```

```cpp
using namespace Windows::ApplicationModel;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;
using namespace AppName;

MainPage::MainPage()
{
   InitializeComponent();
   Application::Current->Suspending +=
       ref new SuspendingEventHandler(this, &MainPage::App_Suspending);
}
```

## <a name="save-application-data-before-suspension"></a>Salvar os dados do aplicativo antes da suspensão

Quando manipula o evento [**Suspending**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.suspending), o aplicativo tem uma oportunidade de salvar seus dados importantes na função do manipulador. O aplicativo deve usar a API de armazenamento [**LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings) para salvar dados simples do aplicativo de maneira síncrona.

```csharp
partial class MainPage
{
    async void App_Suspending(
        Object sender,
        Windows.ApplicationModel.SuspendingEventArgs e)
    {
        // TODO: This is the time to save app data in case the process is terminated.
    }
}
```

```vb
Public NonInheritable Class MainPage

    Private Sub App_Suspending(
        sender As Object,
        e As Windows.ApplicationModel.SuspendingEventArgs) Handles OnSuspendEvent.Suspending

        ' TODO: This is the time to save app data in case the process is terminated.
    End Sub

End Class
```

```cppwinrt
void MainPage::App_Suspending(
    Windows::Foundation::IInspectable const& /* sender */,
    Windows::ApplicationModel::SuspendingEventArgs const& /* e */)
{
    // TODO: This is the time to save app data in case the process is terminated.
}
```

```cpp
void MainPage::App_Suspending(Object^ sender, SuspendingEventArgs^ e)
{
    // TODO: This is the time to save app data in case the process is terminated.
}
```

## <a name="release-resources"></a>Liberar recursos

Você deve liberar recursos e identificadores de arquivo exclusivos para que outros aplicativos tenham acesso a eles enquanto seu aplicativo estiver suspenso. Exemplos de recursos exclusivos incluem câmeras, dispositivos de E/S, dispositivos externos e recursos de rede. Liberar explicitamente os indicadores de arquivos e os recursos exclusivos ajuda a garantir que outros aplicativos possam acessá-los enquanto seu aplicativo estiver suspenso. Quando o aplicativo for retomado, ele deverá readquirir seus identificadores de arquivos e recursos exclusivos.

## <a name="remarks"></a>Comentários

O sistema suspende o aplicativo sempre que o usuário alternar para outro aplicativo, para a área de trabalho ou para a tela inicial. O sistema retoma o seu aplicativo sempre que o usuário alterna de volta para ele. Quando o sistema retoma o aplicativo, o conteúdo das variáveis e estruturas de dados é o mesmo de antes da suspensão do aplicativo pelo sistema. O sistema restaura o aplicativo exatamente como ele havia parado, de maneira que o usuário tem impressão de que ele estava sendo executado em tela de fundo.

O sistema tenta manter o aplicativo e seus dados na memória enquanto ele está suspenso. Entretanto, caso não tenha recursos suficientes para manter o aplicativo na memória, o sistema encerra o aplicativo. Quando o usuário alterna de volta para um aplicativo suspenso que foi encerrado, o sistema envia um evento [**Activated**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) e deve restaurar os dados do aplicativo em seu método [**OnLaunched**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched).

O sistema não notifica um aplicativo quando ele está encerrado, por isso seu aplicativo deve salvar seus dados de aplicativo e liberar identificadores de arquivo e recursos exclusivos quando suspenso e restaurá-los quando ativado após o encerramento.

Se você fizer uma chamada assíncrona no manipulador, o controle será retornado imediatamente daquela chamada assíncrona. Isso significa que a execução pode retornar do manipulador de eventos e o aplicativo mudará para o próximo estado, mesmo que a chamada assíncrona ainda não tenha sido concluída. Use o método [**GetDeferral**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel?redirectedfrom=MSDN) no objeto [**EnteredBackgroundEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel?redirectedfrom=MSDN) que é passado para o manipulador de eventos para atrasar a suspensão até depois que você chamar o método [**Complete**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.complete) no objeto [**Windows.Foundation.Deferral**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral) retornado.

Um adiamento não aumenta a quantidade de código que precisa ser executado antes que o aplicativo seja encerrado. Ele somente atrasa o encerramento até que o método *Complete* do adiamento seja chamado ou o prazo termine, *o que ocorrer primeiro*. Para estender o tempo em que o estado Suspending usa [**ExtendedExecutionSession**](run-minimized-with-extended-execution.md)

> [!NOTE]
> Para melhorar a capacidade de resposta do sistema no Windows 8.1, os aplicativos recebem acesso de baixa prioridade aos recursos depois de serem suspensos. Para dar suporte a essa nova prioridade, o tempo limite da operação de suspensão é estendido para que o aplicativo tenha o equivalente do tempo limite de 5 segundos para prioridade normal no Windows ou entre 1 e 10 segundos no Windows Phone. Você não pode estender ou alterar esse período de tempo limite.

**Observação sobre a depuração com Visual Studio:** o Visual Studio impede que o Windows suspenda um aplicativo que esteja anexado ao depurador. Isso ocorre para permitir que o usuário exiba a interface de usuário de depuração do Visual Studio enquanto o aplicativo está em execução. Quando você está depurando um aplicativo, é possível enviar a ele um evento de suspensão usando o Visual Studio. Verifique se a barra de ferramentas **Local de Depuração** está sendo mostrada e clique no ícone **Suspender**.

## <a name="related-topics"></a>Tópicos relacionados

* [Ciclo de vida do aplicativo](app-lifecycle.md)
* [Tratar a ativação do aplicativo](activate-an-app.md)
* [Tratar a retomada do aplicativo](resume-an-app.md)
* [Diretrizes de UX para iniciar, suspender e retomar](https://docs.microsoft.com/windows/uwp/launch-resume/index)
* [Execução estendida](run-minimized-with-extended-execution.md)

 

 
