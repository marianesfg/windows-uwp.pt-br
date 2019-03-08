---
Description: Ao usar um assistente no Visual Studio, você pode gerar notificações por push a partir de um serviço móvel que foi criado com os Serviços Móveis do Azure.
title: Código gerado pelo assistente de notificação por push
ms.assetid: 340F55C1-0DDF-4233-A8E4-C15EF9030785
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1ac5ca785eab39612bb3a9c6ccd58779c6241059
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596861"
---
# <a name="code-generated-by-the-push-notification-wizard"></a>Código gerado pelo assistente de notificação por push
 

Ao usar um assistente no Visual Studio, você pode gerar notificações por push a partir de um serviço móvel que foi criado com os Serviços Móveis do Azure. O assistente do Visual Studio gera código para ajudá-lo a começar. Este tópico explica como o assistente modifica seu projeto, o que o código gerado faz, como usá-lo e o que pode ser feito em seguida para aproveitar ao máximo as notificações por push. Veja [Visão geral dos Serviços de Notificação por Push do Windows (WNS)](windows-push-notification-services--wns--overview.md).

## <a name="how-the-wizard-modifies-your-project"></a>Como o assistente modifica seu projeto


O assistente de notificação por push modifica seu projeto das seguintes maneiras:

-   Adiciona uma referência ao Cliente Gerenciado dos Serviços Móveis (MobileServicesManagedClient.dll). Não aplicável a projetos JavaScript.
-   Adiciona um arquivo em uma subpasta sob serviços e nomeia o arquivo como push.register.cs, push.register.vb, push.register.cpp ou push.register.js.
-   Cria uma tabela de canais no servidor de banco de dados para o serviço móvel. A tabela contém as informações necessárias para enviar notificações por push a instâncias do aplicativo.
-   Cria scripts para quatro funções: excluir, inserir, ler e atualizar.
-   Cria um script com uma API personalizada, notifyallusers.js, que envia uma notificação por push para todos os clientes.
-   Adiciona uma declaração ao arquivo App.xaml.cs, App.xaml.vb ou App.xaml.cpp, ou adiciona uma declaração a um novo arquivo, service.js, para projetos JavaScript. A declaração declara um objeto MobileServiceClient, que contém as informações necessárias para estabelecer conexão ao serviço móvel. Você pode acessar esse objeto MobileServiceClient, que é chamado *MyServiceName*Client, de qualquer página em seu aplicativo usando o nome App.*MyServiceName*Client.

O arquivo services.js contém o código a seguir:

```js
var <mobile-service-name>Client = new Microsoft.WindowsAzure.MobileServices.MobileServiceClient(
                "https://<mobile-service-name>.azure-mobile.net/",
                "<your client secret>");
```

## <a name="registration-for-push-notifications"></a>Registro para notificações por push


No push.register. \*, o método UploadChannel registra o dispositivo para receber notificações por push. A Loja controla as instâncias instaladas do seu aplicativo e fornece o canal de notificação por push. Consulte [**PushNotificationChannelManager**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager).

O código do cliente é semelhante para o back-end JavaScript e o back-end .NET. Por padrão, quando você adicionar notificações por push para um serviço de back-end JavaScript, uma chamada de amostra para a API personalizada notifyAllUsers será inserida no método UploadChannel.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using Microsoft.WindowsAzure.MobileServices;
using Newtonsoft.Json.Linq;

namespace App2
{
    internal class mymobileservice1234Push
    {
        public async static void UploadChannel()
        {
            var channel = await Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            try
            {
                await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri);
                await App.mymobileservice1234Client.InvokeApiAsync("notifyAllUsers");
            }
            catch (Exception exception)
            {
                HandleRegisterException(exception);
            }
        }

        private static void HandleRegisterException(Exception exception)
        {
            
        }
    }
}
```

```vb
Imports Microsoft.WindowsAzure.MobileServices
Imports Newtonsoft.Json.Linq

Friend Class mymobileservice1234Push
    Public Shared Async Sub UploadChannel()
        Dim channel = Await Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync()

        Try
            Await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri)
            Await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri, New String() {"tag1", "tag2"})
            Await App.mymobileservice1234Client.InvokeApiAsync("notifyAllUsers")
        Catch exception As Exception
            HandleRegisterException(exception)
        End Try
    End Sub

    Private Shared Sub HandleRegisterException(exception As Exception)

    End Sub
End Class
```

```c++
#include "pch.h"
#include "services\mobile services\mymobileservice1234\mymobileservice1234Push.h"

using namespace AzureMobileHelper;

using namespace web;
using namespace concurrency;

using namespace Windows::Networking::PushNotifications;

void mymobileservice1234Push::UploadChannel()
{
    create_task(PushNotificationChannelManager::CreatePushNotificationChannelForApplicationAsync()).
    then([] (PushNotificationChannel^ newChannel) 
    {
        return mymobileservice1234MobileService::GetClient().get_push().register_native(newChannel->Uri->Data());
    }).then([]()
    {
        return mymobileservice1234MobileService::GetClient().invoke_api(L"notifyAllUsers");
    }).then([](task<json::value> result)
    {
        try
        {
            result.wait();
        }
        catch(...)
        {
            HandleExceptionsComingFromTheServer();
        }
    });
}

void mymobileservice1234Push::HandleExceptionsComingFromTheServer()
{
}
```

```js
(function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;

    app.addEventListener("activated", function (args) {
        if (args.detail.kind == activation.ActivationKind.launch) {
            Windows.Networking.PushNotifications.PushNotificationChannelManager.createPushNotificationChannelForApplicationAsync()
                .then(function (channel) {
                    mymobileserviceclient1234Client.push.registerNative(channel.Uri, new Array("tag1", "tag2"))
                    return mymobileservice1234Client.push.registerNative(channel.uri);
                })
                .done(function (registration) {
                    return mymobileservice1234Client.invokeApi("notifyAllUsers");
                }, function (error) {
                    // Error

                });
        }
    });
})();
```

Marcas de notificação por push fornecem uma maneira de restringir as notificações a um subconjunto de clientes. Você pode usar o método registerNative (ou RegisterNativeAsync) para se registrar para todas as notificações por push sem especificar marcas, ou você pode se registrar com marcas, fornecendo o segundo argumento, uma matriz de marcas. Se você se registrar com uma ou mais marcas, só receberá notificações que correspondem às marcas.

## <a name="server-side-scripts-javascript-backend-only"></a>Scripts do lado do servidor (back-end JavaScript somente)


Para os serviços móveis que usam o back-end JavaScript, os scripts do lado do servidor são executados quando ocorrem exclusão, inserção, leitura ou operações de atualização. Os scripts não implementam essas operações, mas são executados quando uma chamada do cliente para a API REST do Windows Mobile dispara esses eventos. Os scripts repassam o controle para as próprias operações chamando request.execute ou request.respond para emitir uma resposta ao contexto de chamada. Consulte [Referência da API REST dos Serviços Móveis do Microsoft Azure](https://go.microsoft.com/fwlink/p/?linkid=511139).

Uma variedade de funções está disponível no script do lado do servidor. Consulte [Registrar operações de tabela nos Serviços Móveis do Azure](https://go.microsoft.com/fwlink/p/?linkid=511140). Para obter uma referência para todas as funções disponíveis, consulte [Referência a scripts de servidor dos Serviços Móveis](https://go.microsoft.com/fwlink/p/?linkid=257676).

O código da API personalizada a seguir em Notifyallusers.js também já foi criado:

```js
exports.post = function(request, response) {
    response.send(statusCodes.OK,{ message : 'Hello World!' })
    
    // The following call is for illustration purpose only
    // The call and function body should be moved to a script in your app
    // where you want to send a notification
    sendNotifications(request);
};

// The following code should be moved to appropriate script in your app where notification is sent
function sendNotifications(request) {
    var payload = '<?xml version="1.0" encoding="utf-8"?><toast><visual><binding template="ToastText01">' +
        '<text id="1">Sample Toast</text></binding></visual></toast>';
    var push = request.service.push; 
    push.wns.send(null,
        payload,
        'wns/toast', {
            success: function (pushResponse) {
                console.log("Sent push:", pushResponse);
            }
        });
}
```

A função sendNotifications envia uma única notificação como notificação do sistema. Você também pode usar outros tipos de notificações por push.

**Dica**  para obter informações sobre como obter a Ajuda durante a edição de scripts, consulte [permitindo o IntelliSense para JavaScript do lado do servidor](https://go.microsoft.com/fwlink/p/?LinkId=309275).

 

## <a name="push-notification-types"></a>Tipos de notificação por push


O Windows dá suporte a notificações que não são notificações por push. Para obter informações gerais sobre notificações, veja [Escolher um método de entrega de notificação](choosing-a-notification-delivery-method.md).

As notificações do sistema são fáceis de usar e você pode revisar um exemplo no código Insert.js na tabela de canais que é gerada para você. Se pretender usar notificações de bloco ou de selo, precisará criar um modelo XML para o bloco e selo, além de precisar especificar a codificação das informações de pacote no modelo. Consulte [Trabalhando com blocos, selos e notificações do sistema](https://msdn.microsoft.com/library/windows/apps/xaml/hh868259).

Como o Windows responde a notificações por push, ele pode manipular a maioria dessas notificações quando o aplicativo está sendo executado. Por exemplo, uma notificação por push poderia avisar um usuário quando uma nova mensagem estivesse disponível mesmo quando o aplicativo de email local não estivesse em execução. O Windows manipula uma notificação do sistema exibindo uma mensagem, como a primeira linha de uma mensagem de texto. O Windows manipula uma notificação de bloco ou notificação simples atualizando o bloco dinâmico de um aplicativo para refletir a quantidade de novas mensagens de email. Dessa maneira, você pode solicitar aos usuários do seu aplicativo que verifiquem se há novas informações. O aplicativo pode receber notificações brutas quando está em execução, e você pode usá-las para enviar dados ao seu aplicativo. Se o aplicativo não estiver em execução, você poderá configurar uma tarefa de segundo plano para monitorar as notificações por push.

Você deve usar as notificações por push de acordo com as diretrizes para os aplicativos da Plataforma Universal do Windows (UWP), porque essas notificações consomem os recursos de um usuário e podem ser traiçoeiras se usadas em excesso. Veja [Diretrizes e lista de verificação de notificações por push](https://msdn.microsoft.com/library/windows/apps/hh761462).

Se você estiver atualizando blocos dinâmicos com notificações por push, também deverá seguir as diretrizes em [Diretrizes e lista de verificação de blocos e notificações](https://msdn.microsoft.com/library/windows/apps/hh465403).

## <a name="next-steps"></a>Próximas etapas


### <a name="using-the-windows-push-notification-services-wns"></a>Usando o WNS (Serviços de Notificação por Push do Windows)

Você poderá chamar os Serviços de Notificação por Push do Windows (WNS) diretamente se os Serviços Móveis não oferecerem flexibilidade suficiente, se quiser escrever o código do servidor em C# ou Visual Basic ou se já tiver um serviço de nuvem e quiser enviar notificações por push a partir dele. Chamando o WNS diretamente, é possível enviar notificações por push de seu próprio serviço de nuvem, como uma função do funcionário que monitora dados de um banco de dados ou de outro serviço da Web. Seu serviço de nuvem precisa se autenticar com WNS para enviar notificações por push aos seus aplicativos. Consulte [Como se autenticar com o Serviço de Notificação por Push do Windows (JavaScript)](https://msdn.microsoft.com/library/windows/apps/hh465407) ou [(C#/C++/VB)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868206).

Você também pode enviar notificações por push executando uma tarefa agendada em seu serviço móvel. Consulte [Agendar trabalhos recorrentes em Serviços Móveis](https://go.microsoft.com/fwlink/p/?linkid=301694).

**Aviso**  após executar o Assistente de notificação por push uma vez, não execute o Assistente uma segunda vez para adicionar o código de registro para outro serviço móvel. Executar o assistente mais de uma vez por projeto gera código que resulta na sobreposição de chamadas para o método [**CreatePushNotificationChannelForApplicationAsync**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync), o que leva a uma exceção de tempo de execução. Se você quiser se registrar para notificações por push em mais de um serviço móvel, execute o assistente uma vez e, em seguida, reescreva o código de registro para garantir que as chamadas para **CreatePushNotificationChannelForApplicationAsync** não sejam executadas ao mesmo tempo. Por exemplo, você pode fazer isso movendo o código gerado pelo assistente no push.register. \* (incluindo a chamada para **CreatePushNotificationChannelForApplicationAsync**) fora do OnLaunched evento, mas as especificações dependem arquitetura do aplicativo.

 

## <a name="related-topics"></a>Tópicos relacionados


* [Visão geral dos Serviços de Notificação por Push do Windows (WNS)](windows-push-notification-services--wns--overview.md)
* [Visão geral de notificação de dados brutos](raw-notification-overview.md)
* [Conectar-se ao Windows serviços móveis do Azure (JavaScript)](https://msdn.microsoft.com/library/windows/apps/dn263160)
* [Conectar-se ao Windows serviços móveis do Azure (C#/C+ c++ /CLI VB)](https://msdn.microsoft.com/library/windows/apps/xaml/dn263175)
* [Guia de início rápido: Adicionar notificações por push para um serviço móvel (JavaScript)](https://msdn.microsoft.com/library/windows/apps/dn263163)
 

 




