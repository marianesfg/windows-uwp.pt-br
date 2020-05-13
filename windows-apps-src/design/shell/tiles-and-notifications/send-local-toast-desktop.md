---
Description: Saiba como os aplicativos Win32 C# podem enviar notificações do sistema local e manipular o usuário clicando no sistema de notificação.
title: Enviar uma notificação do sistema local a partir de aplicativos C# da área de trabalho
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from desktop C# apps
template: detail.hbs
ms.date: 01/23/2018
ms.topic: article
keywords: Windows 10, UWP, Win32, área de trabalho, notificações do sistema, enviar um sistema de notificação, enviar notificação local, desktop Bridge, msix, pacotes esparsos, C#, C sustenido, notificações do sistema, WPF
ms.localizationpriority: medium
ms.openlocfilehash: 679254aa35ea49e72f7feaae02ba0ccbddeafdad
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83233667"
---
# <a name="send-a-local-toast-notification-from-desktop-c-apps"></a>Enviar uma notificação do sistema local a partir de aplicativos C# da área de trabalho

Os aplicativos da área de trabalho (incluindo aplicativos [MSIX](https://docs.microsoft.com/windows/msix/desktop/source-code-overview) empacotados, aplicativos que usam [pacotes esparsos](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) para obter a identidade do pacote e aplicativos Win32 não empacotados clássicos) podem enviar notificações de sistema interativas, assim como os aplicativos do Windows. No entanto, há algumas etapas especiais para aplicativos de desktop devido aos diferentes esquemas de ativação e a possível falta de identidade de pacote se você não estiver usando MSIX ou pacotes esparsos.

> [!IMPORTANT]
> Se você estiver criando um aplicativo UWP, consulte a [documentação da UWP](send-local-toast.md). Para outros idiomas da área de trabalho, consulte [WRL C++ da área de trabalho](send-local-toast-desktop-cpp-wrl.md).


## <a name="step-1-enable-the-windows-runtime-apis"></a>Etapa 1: habilitar as APIs de Windows Runtime

Se você não tiver referenciado as APIs de Windows Runtime de seu aplicativo Win32, deverá fazer isso primeiro.

Basta instalar o `Microsoft.Windows.SDK.Contracts` [pacote NuGet](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) em seu projeto. Saiba mais sobre como [habilitar as APIs de Windows Runtime aqui](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance).


## <a name="step-2-copy-compat-library-code"></a>Etapa 2: copiar o código da biblioteca de compatibilidade

Copie o [arquivo DesktopNotificationManagerCompat.cs do GitHub](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CS/DesktopToastsApp/DesktopNotificationManagerCompat.cs) para seu projeto. A biblioteca de compatibilidade abstrai grande parte da complexidade das notificações da área de trabalho. As instruções a seguir exigem a biblioteca de compatibilidade.


## <a name="step-3-implement-the-activator"></a>Etapa 3: implementar o ativador

Você deve implementar um manipulador para ativação do sistema de forma que, quando o usuário clicar no seu sistema de notificação, seu aplicativo possa fazer algo. Isso é necessário para a notificação do sistema persistir na Central de Ações (pois a notificação do sistema pode ser clicada dias depois quando o aplicativo é fechado). Essa classe pode ser colocada em qualquer lugar do projeto.

Crie uma nova classe **MyNotificationActivator** e estenda a classe **NotificationActivator** . Adicione os três atributos listados abaixo e crie um CLSID GUID exclusivo para seu aplicativo usando um dos muitos geradores de GUID online. Essa CLSID (identificador de classe) é como a Central de Ações sabe qual classe ativar para COM.

**MyNotificationActivator.cs** (criar este arquivo)

```csharp
// The GUID CLSID must be unique to your app. Create a new GUID if copying this code.
[ClassInterface(ClassInterfaceType.None)]
[ComSourceInterfaces(typeof(INotificationActivationCallback))]
[Guid("replaced-with-your-guid-C173E6ADF0C3"), ComVisible(true)]
public class MyNotificationActivator : NotificationActivator
{
    public override void OnActivated(string invokedArgs, NotificationUserInput userInput, string appUserModelId)
    {
        // TODO: Handle activation
    }
}
```


## <a name="step-4-register-with-notification-platform"></a>Etapa 4: registrar-se na plataforma de notificação

Em seguida, você deve se registrar na plataforma de notificação. Há diferentes etapas, dependendo se você estiver usando pacotes MSIX/esparsos ou Win32 clássico. Em caso de suporte para ambos, você deve executar as duas etapas (no entanto, não há necessidade de bifurcar o código; a nossa biblioteca processa isso para você!).


### <a name="msixsparse-packages"></a>Pacotes MSIX/esparsos

Se você estiver usando um pacote [MSIX](https://docs.microsoft.com/windows/msix/desktop/source-code-overview) ou [esparso](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) (ou se você der suporte a ambos), em seu **Package. appxmanifest**, adicione:

1. Declaração para **xmlns:com**
2. Declaração para **xmlns:desktop**
3. No atributo **IgnorableNamespaces**, **com** e **desktop**
4. **com:Extension** do ativador COM usando a GUID da etapa 4. Certifique-se de incluir `Arguments="-ToastActivated"` para saber que a inicialização foi de uma notificação do sistema
5. **Desktop: extensão** para **Windows. toastNotificationActivation** para declarar seu CLSID do torradeira Activator (o GUID da etapa #3).

**Package. appxmanifest**

```xml
<!--Add these namespaces-->
<Package
  ...
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="... com desktop">
  ...
  <Applications>
    <Application>
      ...
      <Extensions>

        <!--Register COM CLSID LocalServer32 registry key-->
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:ExeServer Executable="YourProject\YourProject.exe" Arguments="-ToastActivated" DisplayName="Toast activator">
              <com:Class Id="replaced-with-your-guid-C173E6ADF0C3" DisplayName="Toast activator"/>
            </com:ExeServer>
          </com:ComServer>
        </com:Extension>

        <!--Specify which CLSID to activate when toast clicked-->
        <desktop:Extension Category="windows.toastNotificationActivation">
          <desktop:ToastNotificationActivation ToastActivatorCLSID="replaced-with-your-guid-C173E6ADF0C3" /> 
        </desktop:Extension>

      </Extensions>
    </Application>
  </Applications>
 </Package>
```


### <a name="classic-win32"></a>Win32 clássico

Se você estiver usando o Win32 clássico (ou se você der suporte a ambos), será necessário declarar a AUMID (ID do modelo de usuário do aplicativo) e o CLSID do torradeira Activator (o GUID da etapa #3) no atalho do aplicativo em Iniciar.

Escolha uma AUMID exclusiva para identificar o aplicativo Win32. Em geral, isso ocorre na forma de [CompanyName].[AppName], mas você quer garantir que isso seja exclusivo em todos os aplicativos (fique à vontade para adicionar alguns dígitos no final).

#### <a name="step-41-wix-installer"></a>Etapa 4,1: instalador do WiX

Se estiver usando o WiX como instalador, edite o arquivo **Product.wxs** para adicionar duas propriedades de atalho ao atalho do menu Iniciar, como mostrado abaixo. Verifique se o GUID da etapa #3 está incluído no `{}` , conforme mostrado abaixo.

**Product.wxs**

```xml
<Shortcut Id="ApplicationStartMenuShortcut" Name="Wix Sample" Description="Wix Sample" Target="[INSTALLFOLDER]WixSample.exe" WorkingDirectory="INSTALLFOLDER">
                    
    <!--AUMID-->
    <ShortcutProperty Key="System.AppUserModel.ID" Value="YourCompany.YourApp"/>
    
    <!--COM CLSID-->
    <ShortcutProperty Key="System.AppUserModel.ToastActivatorCLSID" Value="{replaced-with-your-guid-C173E6ADF0C3}"/>
    
</Shortcut>
```

> [!IMPORTANT]
> Para usar as notificações, você deve instalar o aplicativo por meio do instalador uma vez antes da depuração normal para que o atalho de Iniciar com a AUMID e a CLSID esteja presente. Depois que o atalho de Iniciar estiver presente, você pode depurar usando F5 no Visual Studio.


#### <a name="step-42-register-aumid-and-com-server"></a>Etapa 4,2: registrar o AUMID e o servidor COM

Em seguida, independentemente do seu instalador, no código de inicialização do seu aplicativo (antes de chamar qualquer API de notificação), chame o método **RegisterAumidAndComServer** , especificando a classe de seu ativador de notificação da etapa #3 e seu AUMID usado acima.

```csharp
// Register AUMID and COM server (for MSIX/sparse package apps, this no-ops)
DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
```

Se você der suporte ao pacote MSIX/esparso e ao Win32 clássico, sinta-se à vontade para chamar esse método independentemente. Se você estiver executando em um pacote MSIX/esparso, esse método simplesmente retornará imediatamente. Não há necessidade de bifurcar o código.

Esse método permite que você chame as APIs de compatibilidade para enviar e gerenciar notificações sem precisar fornecer constantemente a AUMID. Além disso, insere a chave de registro LocalServer32 do servidor COM.


## <a name="step-5-register-com-activator"></a>Etapa 5: registrar o COM Activator

Para o pacote MSIX/esparso e os aplicativos Win32 clássicos, você deve registrar seu tipo de ativação de notificação para que possa lidar com ativações do sistema.

No código de inicialização do seu aplicativo, chame o seguinte método **RegisterActivator** , passando sua implementação da classe **NotificationActivator** que você criou na etapa #3. É necessário chamá-lo para que você receba quaisquer ativações de notificação do sistema.

```csharp
// Register COM server and activator type
DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();
```


## <a name="step-6-send-a-notification"></a>Etapa 6: Enviar uma notificação

O envio de notificações é idêntico ao dos aplicativos UWP, exceto que você usará a classe **DesktopNotificationManagerCompat** para criar um **ToastNotifier**. A biblioteca de compatibilidade lida automaticamente com a diferença entre o pacote MSIX/esparso e o Win32 clássico, para que você não precise bifurcar seu código. Para o Win32 clássico, a biblioteca de compatibilidade armazena em cache a AUMID fornecida ao chamar **RegisterAumidAndComServer** para que você não precisa se preocupar sobre quando deve fornecer ou não a AUMID.

> [!NOTE]
> Instale a [biblioteca de Notificações](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) para que você possa construir notificações usando C# como mostrado abaixo, em vez de usar XML bruto.

Certifique-se de usar o **ToastContent** visto abaixo (ou o modelo ToastGeneric se você estiver criando XML de mão), já que os modelos de notificação do sistema herdado Windows 8.1 não ativarão o seu ativador de notificação com criado na etapa #3.

> [!IMPORTANT]
> As imagens http têm suporte apenas em aplicativos de pacote MSIX/esparsos que têm o recurso de Internet em seu manifesto. Os aplicativos do Win32 clássicos não oferecem suporte às imagens http; você deve baixar a imagem para os dados de aplicativo local e referenciá-los localmente.

```csharp
// Construct the visuals of the toast (using Notifications library)
ToastContent toastContent = new ToastContent()
{
    // Arguments when the user taps body of toast
    Launch = "action=viewConversation&conversationId=5",

    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Hello world!"
                }
            }
        }
    }
};

// Create the XML document (BE SURE TO REFERENCE WINDOWS.DATA.XML.DOM)
var doc = new XmlDocument();
doc.LoadXml(toastContent.GetContent());

// And create the toast notification
var toast = new ToastNotification(doc);

// And then show it
DesktopNotificationManagerCompat.CreateToastNotifier().Show(toast);
```

> [!IMPORTANT]
> Os aplicativos clássicos do Win32 não podem usar modelos de notificação do sistema herdados (como ToastText02). A ativação dos modelos herdados falha quando CLSID COM é especificada. Você deve usar os modelos ToastGeneric do Windows 10 conforme visto acima.


## <a name="step-7-handling-activation"></a>Etapa 7: manipulando a ativação

Quando o usuário clica na notificação do sistema, o método **OnActivated** da classe **NotificationActivator** é invocado.

No método OnActivated, você pode analisar os argumentos especificados na notificação do sistema e obter a entrada que o usuário digitou ou selecionou e, em seguida, ativar o aplicativo adequadamente.

> [!NOTE]
> O método **OnActivated** não é chamado no thread da interface do usuário. Se quiser executar operações de thread da interface do usuário, você deve chamar `Application.Current.Dispatcher.Invoke(callback)`.

```csharp
// The GUID must be unique to your app. Create a new GUID if copying this code.
[ClassInterface(ClassInterfaceType.None)]
[ComSourceInterfaces(typeof(INotificationActivationCallback))]
[Guid("replaced-with-your-guid-C173E6ADF0C3"), ComVisible(true)]
public class MyNotificationActivator : NotificationActivator
{
    public override void OnActivated(string invokedArgs, NotificationUserInput userInput, string appUserModelId)
    {
        Application.Current.Dispatcher.Invoke(delegate
        {
            // Tapping on the top-level header launches with empty args
            if (arguments.Length == 0)
            {
                // Perform a normal launch
                OpenWindowIfNeeded();
                return;
            }

            // Parse the query string (using NuGet package QueryString.NET)
            QueryString args = QueryString.Parse(invokedArgs);

            // See what action is being requested 
            switch (args["action"])
            {
                // Open the image
                case "viewImage":

                    // The URL retrieved from the toast args
                    string imageUrl = args["imageUrl"];

                    // Make sure we have a window open and in foreground
                    OpenWindowIfNeeded();

                    // And then show the image
                    (App.Current.Windows[0] as MainWindow).ShowImage(imageUrl);

                    break;

                // Background: Quick reply to the conversation
                case "reply":

                    // Get the response the user typed
                    string msg = userInput["tbReply"];

                    // And send this message
                    SendMessage(msg);

                    // If there's no windows open, exit the app
                    if (App.Current.Windows.Count == 0)
                    {
                        Application.Current.Shutdown();
                    }

                    break;
            }
        });
    }

    private void OpenWindowIfNeeded()
    {
        // Make sure we have a window open (in case user clicked toast while app closed)
        if (App.Current.Windows.Count == 0)
        {
            new MainWindow().Show();
        }

        // Activate the window, bringing it to focus
        App.Current.Windows[0].Activate();

        // And make sure to maximize the window too, in case it was currently minimized
        App.Current.Windows[0].WindowState = WindowState.Normal;
    }
}
```

Para oferecer a inicialização correta do suporte enquanto o aplicativo é fechado, no arquivo `App.xaml.cs` você deve substituir o método **OnStartup** (para aplicativos WPF) para determinar se está sendo iniciado em uma notificação do sistema ou não. Se for iniciado a partir de uma notificação do sistema, haverá um arg de inicialização de "-ToastActivated". Ao observar isso, você deve parar de executar qualquer código de ativação de inicialização normal e permitir que o código **OnActivated** processe o lançamento.

```csharp
protected override async void OnStartup(StartupEventArgs e)
{
    // Register AUMID, COM server, and activator
    DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
    DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();

    // If launched from a toast
    if (e.Args.Contains("-ToastActivated"))
    {
        // Our NotificationActivator code will run after this completes,
        // and will show a window if necessary.
    }

    else
    {
        // Show the window
        // In App.xaml, be sure to remove the StartupUri so that a window doesn't
        // get created by default, since we're creating windows ourselves (and sometimes we
        // don't want to create a window if handling a background activation).
        new MainWindow().Show();
    }

    base.OnStartup(e);
}
```


### <a name="activation-sequence-of-events"></a>Sequência de ativação de eventos

Para WPF, a sequência de ativação é a seguinte...

Se o aplicativo já estiver sendo executado:

1. **OnActivated** no **NotificationActivator** é chamada

Se o aplicativo não estiver sendo executado:

1. **OnStartup** em `App.xaml.cs` é chamado com **Args** de "-ToastActivated"
2. **OnActivated** no **NotificationActivator** é chamada


### <a name="foreground-vs-background-activation"></a>Ativação em primeiro plano e segundo plano
Para aplicativos da área de trabalho, a ativação em primeiro plano e em segundo plano é processada de forma idêntica: o ativador COM é chamado. O código do aplicativo deve decidir se deseja mostrar uma janela ou apenas executar algum trabalho e sair. Portanto, a especificação de um **ActivationType** de **plano de fundo** no conteúdo do sistema não altera o comportamento.


## <a name="step-8-remove-and-manage-notifications"></a>Etapa 8: remover e gerenciar notificações

A remoção e o gerenciamento de notificações é idêntico ao dos aplicativos UWP. No entanto, recomendamos que você use a biblioteca de compatibilidade para obter uma **DesktopNotificationHistoryCompat** para que você não precisa se preocupar em fornecer a AUMID se estiver usando o Win32 clássico.

```csharp
// Remove the toast with tag "Message2"
DesktopNotificationManagerCompat.History.Remove("Message2");

// Clear all toasts
DesktopNotificationManagerCompat.History.Clear();
```


## <a name="step-9-deploying-and-debugging"></a>Etapa 9: Implantando e Depurando

Para implantar e depurar seu aplicativo MSIX, consulte [Executar, depurar e testar um aplicativo de área de trabalho empacotado](/windows/uwp/porting/desktop-to-uwp-debug).

Para implantar e depurar seu aplicativo para Win32 clássico, você deve instalar o aplicativo pelo instalador uma vez antes da depuração normal para que o atalho no menu Iniciar com a AUMID e a CLSID esteja presente. Depois que o atalho de Iniciar estiver presente, você pode depurar usando F5 no Visual Studio.

Caso suas notificações simplesmente não apareçam no aplicativo para Win32 clássico (e nenhuma exceção for lançada), isso provavelmente significa que o atalho de Iniciar não está presente (instalação do aplicativo pelo instalador) ou a AUMID usada no código não corresponde à AUMID do atalho do menu Iniciar.

Se as notificações aparecem, mas não continuam na Central de Ações (desaparecem após ignorar o pop-up), isso significa que você ainda não implementou o ativador COM corretamente.

Se você tiver instalado o pacote MSIX/esparso e o aplicativo Win32 clássico, observe que o aplicativo de pacote MSIX/esparso substituirá o aplicativo Win32 clássico ao lidar com ativações do sistema. Isso significa que as notificações do aplicativo Win32 clássico ainda iniciarão o aplicativo de pacote MSIX/esparso quando clicado. A desinstalação do aplicativo de pacote MSIX/esparso reverterá as ativações de volta para o aplicativo Win32 clássico.


## <a name="known-issues"></a>Problemas conhecidos

**CORRIGIDO: o aplicativo não é concentrado depois de clicar na notificação do sistema**: nas versões 15063 e anteriores, os direitos de primeiro plano não estavam sendo transferidos para o aplicativo ao ativar o servidor COM. Portanto,o aplicativo simplesmente piscava ao tentar movê-lo para o primeiro plano. Não havia uma solução alternativa para esse problema. Corrigimos isso nas versões 16299 e posteriores.


## <a name="resources"></a>Recursos

* [Exemplo de código completo em GitHub](https://github.com/WindowsNotifications/desktop-toasts)
* [Notificações do sistema a partir de aplicativos da área de trabalho](toast-desktop-apps.md)
* [Conteúdo e documentação sobre notificações do sistema](adaptive-interactive-toasts.md)

