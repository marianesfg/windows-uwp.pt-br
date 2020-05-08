---
title: Canais de push alternativos usando VAPID no UWP
description: Instruções para usar canais de push alternativos com o protocolo VAPID de um aplicativo do Windows
ms.date: 01/10/2017
ms.topic: article
keywords: Windows 10, UWP, API do WinRT, WNS
localizationpriority: medium
ms.openlocfilehash: 382dca376e2393d83c2803043b61db76226b3995
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970861"
---
# <a name="alternate-push-channels-using-vapid-in-windows"></a>Canais de push alternativos usando VAPID no Windows 
A partir da atualização de criadores de outono, os aplicativos do Windows podem usar a autenticação VAPID para enviar notificações por push.  

> [!NOTE]
> Essas APIs são destinadas a navegadores da Web que hospedam outros sites e a criação de canais em seu nome.  Se você estiver procurando adicionar notificações do webpush ao seu aplicativo Web, recomendamos seguir os padrões W3C e WhatWG para criar um trabalhador de serviço e enviar uma notificação.

## <a name="introduction"></a>Introdução
A introdução do padrão de push da Web permite que os sites possam agir mais como aplicativos, enviando notificações mesmo quando os usuários não estão no site.

O protocolo de autenticação VAPID foi criado para permitir que sites se autentiquem com servidores de push de maneira independente de fornecedor. Com todos os fornecedores que usam o protocolo VAPID, os sites podem enviar notificações por push sem saber o navegador no qual ele está sendo executado. Essa é uma melhoria significativa em relação à implementação de um protocolo de push diferente para cada plataforma. 

Os aplicativos do Windows também podem usar o VAPID para enviar notificações por push com essas vantagens. Esses protocolos podem economizar tempo de desenvolvimento para novos aplicativos e simplificar o suporte de plataforma cruzada para aplicativos existentes. Além disso, aplicativos empresariais ou aplicativos Sideload agora podem enviar notificações sem se registrar no Microsoft Store. Espero que isso abra novas maneiras de se envolver com os usuários em todas as plataformas.  

## <a name="alternate-channels"></a>Canais alternativos 
No UWP, esses canais VAPID são chamados de canais alternativos e fornecem funcionalidade semelhante a um canal de push da Web. Eles podem disparar a execução de uma tarefa em segundo plano do aplicativo, habilitar a criptografia de mensagens e permitir vários canais de um único aplicativo. Para obter mais informações sobre a diferença entre os diferentes tipos de canal, consulte [escolher o canal correto](channel-types.md).

Usar canais alternativos é uma ótima maneira de acessar notificações por Push se seu aplicativo não puder usar um canal primário ou se você quiser compartilhar código entre seu site e aplicativo. A configuração de um canal é fácil e familiar para qualquer pessoa que tenha usado o padrão de push da Web ou trabalhou com notificações por push do Windows antes.

## <a name="code-example"></a>Exemplo de código

O processo básico de configuração de um canal alternativo para um aplicativo do Windows é semelhante à configuração de um canal primário ou secundário. Primeiro, registre-se para um canal com o [servidor WNS](windows-push-notification-services--wns--overview.md). Em seguida, registre-se para executar como uma tarefa em segundo plano. Depois que a notificação for enviada e a tarefa em segundo plano for disparada, manipule o evento.  

### <a name="get-a-channel"></a>Obter um canal 
Para criar um canal alternativo, o aplicativo deve fornecer duas informações: a chave pública para seu servidor e o nome do canal que está sendo criado. Os detalhes sobre as chaves de servidor estão disponíveis na especificação de push da Web, mas é recomendável usar uma biblioteca de push da Web padrão no servidor para gerar as chaves.  

A ID do canal é particularmente importante porque um aplicativo pode criar vários canais alternativos. Cada canal deve ser identificado por uma ID exclusiva que será incluída com quaisquer cargas de notificação enviadas ao longo desse canal.  

```csharp
private async void AppCreateVAPIDChannelAsync(string appChannelId, IBuffer applicationServerKey) 
{ 
    // From the spec: applicationServer Key (p256dh):  
    //               An Elliptic curve Diffie–Hellman public key on the P-256 curve 
    //               (that is, the NIST secp256r1 elliptic curve).   
    //               The resulting key is an uncompressed point in ANSI X9.62 format             
    // ChannelId is an app provided value for it to identify the channel later.  
    // case of this app it is from the set { "Football", "News", "Baseball" } 
    PushNotificationChannel webChannel = await PushNotificationChannelManager.GetDefault().CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId); 
 
    //Save the channel  
    AppUpdateChannelMapping(appChannelId, webChannel); 
             
    //Tell our web service that we have a new channel to push notifications to 
    AppPassChannelToSite(webChannel.Uri); 
} 
```
O aplicativo envia o backup do canal para seu servidor e o salva localmente. Salvar a ID do canal localmente permite que o aplicativo diferencie os canais e renove os canais para impedir que eles expirem.

Assim como todos os outros tipos de canal de notificação por push, os canais da Web podem expirar. Para evitar que os canais expirem sem que seu aplicativo saiba, crie um novo canal toda vez que seu aplicativo for iniciado.    

### <a name="register-for-a-background-task"></a>Registrar-se para uma tarefa em segundo plano 

Depois que o aplicativo tiver criado um canal alternativo, ele deverá se registrar para receber as notificações em primeiro plano ou em segundo plano. O exemplo a seguir registra para usar o modelo de um processo para receber as notificações em segundo plano.  

```csharp
var builder = new BackgroundTaskBuilder(); 
builder.Name = "Push Trigger"; 
builder.SetTrigger(new PushNotificationTrigger()); 
BackgroundTaskRegistration task = builder.Register(); 
```
### <a name="receive-the-notifications"></a>Receber as notificações 

Depois que o aplicativo for registrado para receber as notificações, ele precisará ser capaz de processar as notificações de entrada. Como um único aplicativo pode registrar vários canais, certifique-se de verificar a ID do canal antes de processar a notificação.  

```csharp
protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args) 
{ 
    base.OnBackgroundActivated(args); 
    var raw = args.TaskInstance.TriggerDetails as RawNotification; 
    switch (raw.ChannelId) 
    { 
        case "Football": 
            AppPostFootballScore(raw.Content); 
            break; 
        case "News": 
            AppProcessNewsItem(raw.Content); 
            break; 
        case "Baseball": 
            AppProcessBaseball(raw.Content); 
            break; 
 
        default: 
            //We don't have the channelID registered, should only happen in the case of a 
            //caching issue on the application server 
            break; 
    }                           
} 
```

Observe que, se a notificação for proveniente de um canal primário, a ID do canal não será definida.  

## <a name="note-on-encryption"></a>Observação sobre criptografia 

Você pode usar qualquer esquema de criptografia que achar mais útil para seu aplicativo. Em alguns casos, é suficiente contar com o padrão TLS entre o servidor e qualquer dispositivo Windows. Em outros casos, pode fazer mais sentido usar o esquema de criptografia por push da Web ou outro esquema de seu design.  

Se você quiser usar outra forma de criptografia, a chave será usar o RAW. Propriedade Headers. Ele contém todos os cabeçalhos de criptografia que foram incluídos na solicitação POST para o servidor de envio por push. A partir daí, seu aplicativo pode usar as chaves para descriptografar a mensagem.  

## <a name="related-topics"></a>Tópicos relacionados
- [Tipos de canal de notificação](channel-types.md)
- [Serviços de Notificação por Push do Windows (WNS)](windows-push-notification-services--wns--overview.md)
- [Classe PushNotificationChannel](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannel)
- [Classe PushNotificationChannelManager](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager)


