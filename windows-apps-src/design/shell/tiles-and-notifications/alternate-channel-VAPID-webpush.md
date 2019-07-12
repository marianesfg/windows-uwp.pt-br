---
title: Canais de push alternativo usando VAPID na UWP
description: Instruções para usar os canais de push alternativo com o protocolo VAPID de um aplicativo UWP
ms.date: 01/10/2017
ms.topic: article
keywords: Windows 10, uwp, API do WinRT, WNS
localizationpriority: medium
ms.openlocfilehash: 6512eb891967b6c17bc4845d5e47639ae3c97d31
ms.sourcegitcommit: 0c97c025d751082db3424cb9941bf6688d9b7381
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67835020"
---
# <a name="alternate-push-channels-using-vapid-in-uwp"></a>Canais de push alternativo usando VAPID na UWP 
A partir o Fall Creators Update, aplicativos UWP podem usar a autenticação VAPID para enviar notificações por push.  

> [!NOTE]
> Essas APIs são destinadas para navegadores da web que estão hospedando outros sites e criar canais em seu nome.  Se você estiver procurando para adicionar notificações webpush ao seu aplicativo web, é recomendável que você siga os padrões W3C e WhatWG para criar um trabalho de serviço e enviar uma notificação.

## <a name="introduction"></a>Introdução
A introdução do padrão de envio por push da web permite que sites podem funcionar mais como aplicativos, mesmo quando os usuários não estão no site do envio de notificações.

O protocolo de autenticação VAPID foi criado para permitir que os sites autenticar com os servidores de envio por push em um fornecedor de maneira independente. Com todos os fornecedores usando o protocolo VAPID, sites podem enviar notificações por push sem conhecer o navegador no qual ele está em execução. Isso é uma melhoria significativa sobre a implementação de um protocolo de envio por push diferentes para cada plataforma. 

Aplicativos UWP podem usar VAPID para enviar notificações por push com essas vantagens. Esses protocolos podem economizar tempo de desenvolvimento para novos aplicativos e simplificar o suporte de plataforma cruzada para aplicativos existentes. Além disso, aplicativos corporativos ou aplicativos com sideload agora podem enviar notificações sem registrar em que a Microsoft Store. Felizmente, isso abrirá novas maneiras de interagir com usuários em todas as plataformas.  

## <a name="alternate-channels"></a>Canais alternativos 
Na UWP, esses canais VAPID são chamados de canais alternativos em fornecem funcionalidade semelhante a um canal de push da web. Eles podem disparar uma tarefa de plano de fundo do aplicativo para executar, habilitar a criptografia de mensagem e permitir vários canais de um único aplicativo. Para obter mais informações sobre a diferença entre os tipos de canal diferente, consulte [escolhendo o canal corretos](channel-types.md).

Usar canais alternativos é uma ótima maneira de acessar as notificações por push se seu aplicativo não é possível usar um canal principal ou se você quiser compartilhar código entre seu site e aplicativo. Como configurar um canal é fácil e familiar para qualquer pessoa que tenha usado o padrão de envio por push da web ou trabalhou com Windows notificações por push antes.

## <a name="code-example"></a>Exemplo de código

O processo básico de como configurar um canal alternativo para um aplicativo UWP é semelhante à configuração de um canal primário ou secundário. Primeiro, registre-se para um canal com o [server WNS](windows-push-notification-services--wns--overview.md). Em seguida, registre-se para ser executado como uma tarefa em segundo plano. Depois que a notificação é enviada e a tarefa em segundo plano é disparada, manipule o evento.  

### <a name="get-a-channel"></a>Obter um canal 
Para criar um canal alternativo, o aplicativo deve fornecer duas informações: a chave pública do seu servidor e o nome do canal, ele está criando. Os detalhes sobre as chaves de servidor estão disponíveis nas especificações de envio por push da web, mas é recomendável usar uma biblioteca de push padrão da web no servidor para gerar as chaves.  

A ID do canal é particularmente importante porque um aplicativo pode criar diversos canais alternativos. Cada canal deve ser identificado por uma ID exclusiva que será incluída com qualquer cargas de notificação enviadas junto esse canal.  

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
O aplicativo envia o canal de volta até seu servidor e a salva localmente. Salvando a identificação do canal localmente, permite que o aplicativo diferenciar entre os canais e canais de renovação para impedir que eles expirem.

Como cada outro tipo de canal de notificação por push, canais da web podem expirar. Para impedir que os canais expirando sem saber seu aplicativo, crie um novo canal sempre que seu aplicativo é iniciado.    

### <a name="register-for-a-background-task"></a>Registre-se para uma tarefa em segundo plano 

Quando seu aplicativo tiver criado um canal alternativo, ele deve se registrar para receber as notificações em primeiro plano ou segundo plano. O exemplo a seguir registra para usar o modelo de um processo para receber as notificações na tela de fundo.  

```csharp
var builder = new BackgroundTaskBuilder(); 
builder.Name = "Push Trigger"; 
builder.SetTrigger(new PushNotificationTrigger()); 
BackgroundTaskRegistration task = builder.Register(); 
```
### <a name="receive-the-notifications"></a>Receber as notificações 

Depois que o aplicativo tenha sido registrado para receber as notificações, ele precisa ser capaz de processar as notificações recebidas. Uma vez que um único aplicativo pode registrar vários canais, certifique-se de verificar a ID do canal antes de processar a notificação.  

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

Observe que se a notificação é proveniente de um canal primário, em seguida, a ID do canal não será definida.  

## <a name="note-on-encryption"></a>Observe na criptografia 

Você pode usar qualquer esquema de criptografia você considere mais útil para seu aplicativo. Em alguns casos, é suficiente para contar com o padrão TLS entre o servidor e qualquer dispositivo do Windows. Em outros casos, pode fazer mais sentido usar o esquema de criptografia de envio por push da web ou outro esquema de seu design.  

Se você quiser usar outra forma de criptografia, a chave é o uso bruto. Propriedade Headers. Ele contém todos os cabeçalhos de criptografia que foram incluídos na solicitação POST para o servidor de envio por push. A partir daí, seu aplicativo pode usar as chaves para descriptografar a mensagem.  

## <a name="related-topics"></a>Tópicos relacionados
- [Tipos de canais de notificação](channel-types.md)
- [Serviços de notificação por Push do Windows (WNS)](windows-push-notification-services--wns--overview.md)
- [Classe PushNotificationChannel](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannel)
- [Classe PushNotificationChannelManager](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager)


