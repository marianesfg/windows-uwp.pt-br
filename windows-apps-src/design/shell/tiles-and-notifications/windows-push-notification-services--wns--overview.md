---
author: mijacobs
Description: The Windows Push Notification Services (WNS) enables third-party developers to send toast, tile, badge, and raw updates from their own cloud service. This provides a mechanism to deliver new updates to your users in a power-efficient and dependable way.
title: Visão geral do WNS (Serviço de Notificação por Push do Windows)
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2b7d9adfd9e058d4364470b07ef3e9129ade88b3
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5979091"
---
# <a name="windows-push-notification-services-wns-overview"></a>Visão geral dos Serviços de Notificação por Push do Windows (WNS)
 

Os Serviços de Notificação por Push do Windows (WNS) permitem que desenvolvedores terceirizados enviem atualizações de notificações do sistema, de blocos, de selos e brutas pelo próprio serviço de nuvem. Isso proporciona um mecanismo para entregar novas atualizações aos usuários de forma eficaz e confiável.

## <a name="how-it-works"></a>Como funciona


O diagrama a seguir mostra o fluxo de dados completo para o envio de uma notificação por push. Ele envolve estas etapas:

1.  O aplicativo solicita um canal de notificação por push na Plataforma Universal do Windows.
2.  O Windows pede para o WNS criar um canal de notificação. Esse canal é retornado para o dispositivo de chamada sob a forma de um URI.
3.  O URI do canal de notificações é retornado pelo Windows para seu aplicativo.
4.  Seu aplicativo envia o URI para o seu próprio serviço de nuvem. Em seguida, armazene o URI no próprio serviço de nuvem de maneira que seja possível acessar o URI quando você envia as notificações. O URI é uma interface entre o próprio aplicativo e o próprio serviço; é sua responsabilidade implementar essa interface com padrões de segurança e Web segura.
5.  Quando o serviço de nuvem tem uma atualização para envio, ele notifica o WNS usando o URI do canal. Isso é feito por meio da emissão de uma solicitação HTTP POST, incluindo a carga da notificação, sobre o protocolo SSL. Esta etapa requer autenticação.
6.  O WNS recebe a solicitação e encaminha a notificação para o dispositivo apropriado.

![diagrama de fluxo de dados do wns para notificação por push](images/wns-diagram-01.png)

## <a name="registering-your-app-and-receiving-the-credentials-for-your-cloud-service"></a>Registrando seu aplicativo e recebendo as credenciais para o serviço na nuvem


Antes de enviar notificações usando o WNS, o aplicativo deve ser registrado com o Painel da Microsoft Store. Isso lhe fornecerá credenciais para o aplicativo que serão usadas pelo serviço na nuvem para a autenticação no WNS. Essas credenciais consistem em um SID (Identificador de Segurança de Pacote) e uma chave secreta. Para realizar esse registro, vá para o [Centro de Desenvolvimento do Windows](http://go.microsoft.com/fwlink/p/?linkid=511146) e selecione **Painel**. Depois de criar o aplicativo, você pode recuperar as credenciais seguindo as instruções na página de **Gerenciamento de aplicativo - WNS/MPNS**. Se você desejar usar a solução dos serviços Live, siga o link **site de serviços do Live** nesta página.

Cada aplicativo tem seu próprio conjunto de credenciais para seu serviço na nuvem. Essas credenciais não podem ser usadas para enviar notificações para qualquer outro aplicativo.

Para obter mais detalhes sobre como registrar seu aplicativo, consulte o tópico sobre [como autenticar no WNS (Serviço de Notificação do Windows)](https://msdn.microsoft.com/library/windows/apps/hh465407).

## <a name="requesting-a-notification-channel"></a>Solicitando um canal de notificação


Quando um aplicativo que é capaz de receber notificações por push é executado, ele deve primeiro solicitar um canal de notificação por meio do [**CreatePushNotificationChannelForApplicationAsync**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager#Windows_Networking_PushNotifications_PushNotificationChannelManager_CreatePushNotificationChannelForApplicationAsync_System_String_). Para ver uma discussão completa e o exemplo de código, consulte [Como solicitar, criar e salvar um canal de notificação](https://msdn.microsoft.com/library/windows/apps/hh465412). Essa API retorna um URI de canal que está associado exclusivamente ao aplicativo de chamada e seu bloco e pelo qual todos os tipos de notificação podem ser enviados.

Depois o aplicativo cria com êxito um URI de canal, ele o envia para seu serviço na nuvem, juntamente com quaisquer metadados específicos ao aplicativo que devem ser associados a esse URI.

### <a name="important-notes"></a>Observações importantes

-   Não podemos garantir que o URI do canal de notificação de um aplicativo permanecerá sempre o mesmo. Aconselhamos que o aplicativo solicite um novo canal a cada vez que for executado e atualize seu serviço quando o URI for alterado. O desenvolvedor nunca deve modificar o URI do canal e deve considerá-lo como uma cadeia de caracteres de caixa preta. Nesse momento, os URIs do canal expiram após 30 dias. Se seu aplicativo do Windows 10 renovar periodicamente o canal em segundo plano, em seguida, você pode baixar a [amostra de notificações periódicas e por Push](http://go.microsoft.com/fwlink/p/?linkid=231476) para Windows 8.1 e reutilizar o código-fonte e/ou o padrão que ele demonstra.
-   A interface entre o serviço de nuvem e o aplicativo cliente é implementada por você, o desenvolvedor. Recomendamos que o aplicativo passe por um processo de autenticação com o seu próprio serviço e transmita dados por meio de um protocolo seguro, como HTTPS.
-   É importante que o serviço na nuvem sempre garanta que o URI do canal use o domínio "notify.windows.com". O serviço nunca deve enviar as notificações por push para um canal em algum outro domínio. Se o retorno de chamada para o aplicativo fosse comprometido, um invasor mal-intencionado poderia enviar um URI de canal para falsificar o WNS. Sem inspecionar o domínio, o serviço na nuvem poderia revelar as informações a este invasor de forma inconsciente.
-   Se o seu serviço na nuvem tentar entregar uma notificação para um canal expirado, o WNS retornará um [código de resposta 410](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#WNSResponseCodes). Em resposta a esse código, seu serviço não deverá mais tentar enviar notificações a esse URI.

## <a name="authenticating-your-cloud-service"></a>Autenticando seu serviço na nuvem


Para enviar uma notificação, o serviço na nuvem deve ser autenticado por meio do WNS. A primeira etapa neste processo ocorre quando você registra o aplicativo no Painel da Microsoft Store. Durante o processo de registro, o aplicativo recebe um SID (Identificador do Pacote de Segurança) e uma chave secreta. Estas informações são usadas pelo serviço na nuvem para autenticar no WNS.

O esquema de autenticação do WNS é implementado usando o perfil de credenciais de cliente do protocolo [OAuth 2.0](http://go.microsoft.com/fwlink/p/?linkid=226787). O serviço na nuvem autentica no WNS fornecendo suas credenciais (SID do pacote e chave secreta). Em troca, ele recebe um token de acesso. Esse token de acesso permite que um serviço na nuvem envie uma notificação. O token é necessário a cada solicitação de notificação enviada para o WNS.

Um nível elevado, a cadeia de informações é a seguinte:

1.  O serviço na nuvem envia suas credenciais para o WNS sobre HTTPS seguindo o protocolo OAuth 2.0. Isso autentica o serviço no WNS.
2.  O WNS retorna um token de acesso quando a autenticação é bem-sucedida. Este token de acesso é usado nas solicitações de notificações subsequentes até expirar.

![diagrama wns para autenticação de serviço de nuvem](images/wns-diagram-02.png)

Na autenticação no WNS, o serviço na nuvem envia uma solicitação HTTP sobre o protocolo SSL. Os parâmetros são fornecidos no formato "application/x-www-for-urlencoded". Forneça seu SID de pacote no campo "client\_id" e sua chave secreta no campo "client\_secret". Para obter os detalhes da sintaxe, consulte a referência para a [solicitação de token de acesso](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#access_token_request).

**Observação**este é apenas um exemplo, o código não recortar e colar que você pode usar com êxito em seu próprio código.

 

``` http
 POST /accesstoken.srf HTTP/1.1
 Content-Type: application/x-www-form-urlencoded
 Host: https://login.live.com
 Content-Length: 211
 
 grant_type=client_credentials&client_id=ms-app%3a%2f%2fS-1-15-2-2972962901-2322836549-3722629029-1345238579-3987825745-2155616079-650196962&client_secret=Vex8L9WOFZuj95euaLrvSH7XyoDhLJc7&scope=notify.windows.com
```

O WNS autentica o serviço na nuvem e, em caso de êxito, envia uma resposta de "200 OK". O token de acesso é retornado nos parâmetros incluídos no corpo da resposta HTTP, usando o tipo de mídia "application/json". Depois que o seu serviço recebe o token de acesso, você está pronto para enviar notificações.

O exemplo a seguir mostra uma resposta de autenticação bem-sucedida, incluindo o token de acesso. Para obter os detalhes da sintaxe, veja [Cabeçalhos de solicitação e resposta do serviço de notificação por push](https://msdn.microsoft.com/library/windows/apps/hh465435).

``` http
 HTTP/1.1 200 OK   
 Cache-Control: no-store
 Content-Length: 422
 Content-Type: application/json
 
 {
     "access_token":"EgAcAQMAAAAALYAAY/c+Huwi3Fv4Ck10UrKNmtxRO6Njk2MgA=", 
     "token_type":"bearer"
 }
```

### <a name="important-notes"></a>Observações importantes

-   O protocolo OAuth 2.0 com suporte neste procedimento segue a versão de rascunho V16.
-   A RFC (Request for Comments) do OAuth usa o termo "client" para se referir ao serviço na nuvem.
-   Poderá haver alterações neste procedimento quando o rascunho do OAuth for finalizado.
-   O token de acesso pode ser reutilizado em várias solicitações de notificação. Isso permite que o serviço na nuvem autentique somente uma vez para enviar muitas notificações. No entanto, quando o token de acesso expira, o serviço na nuvem deve autenticar novamente para receber um novo token de acesso.

## <a name="sending-a-notification"></a>Enviando uma notificação


Usando o URI do canal, o serviço na nuvem pode enviar uma notificação sempre que tiver uma atualização para o usuário.

O token de acesso descrito acima pode ser reutilizado em várias solicitações de notificação; o servidor na nuvem não é necessário para solicitar um novo token de acesso para cada notificação. Se o token de acesso tiver expirado, a solicitação de notificação retornará um erro. Recomendamos que você não tente reenviar a notificação mais de uma vez se o token de acesso for rejeitado. Se você encontrar esse erro, precisará solicitar um novo token de acesso e reenviar a notificação. Para obter o código de erro exato, consulte os [códigos de resposta da notificação por push](https://msdn.microsoft.com/library/windows/apps/hh465435).

1.  O serviço na nuvem cria um HTTP POST para o URI do canal. Esta solicitação deve ser feita sobre SSL e contém os cabeçalhos e a carga de notificação necessárias. O cabeçalho de autorização deve incluir o símbolo de acesso obtido para a autorização.

    Um exemplo da solicitação é mostrado aqui: Para obter os detalhes da sintaxe, consulte os [códigos de resposta da notificação por push](https://msdn.microsoft.com/library/windows/apps/hh465435).

    Os detalhes sobre como compor a carga da notificação estão em [Guia de início rápido: enviando uma notificação por push](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252). A carga de uma notificação por push de bloco, do sistema ou de selo é fornecida como conteúdo XML que segue seus respectivos [esquemas de blocos adaptáveis](adaptive-tiles-schema.md) ou [esquemas de blocos herdados](https://msdn.microsoft.com/library/windows/apps/br212853) definidos. A carga de uma notificação bruta não tem uma estrutura especificada. Ela é estritamente definida pelo aplicativo.

    ``` http
     POST https://cloud.notify.windows.com/?token=AQE%bU%2fSjZOCvRjjpILow%3d%3d HTTP/1.1
     Content-Type: text/xml
     X-WNS-Type: wns/tile
     Authorization: Bearer EgAcAQMAAAAALYAAY/c+Huwi3Fv4Ck10UrKNmtxRO6Njk2MgA=
     Host: cloud.notify.windows.com
     Content-Length: 24

     <body>
     ....
    ```

2.  O WNS responde para indicar que a notificação foi recebida e será entregue na próxima oportunidade disponível. No entanto, o WNS não fornece a confirmação completa de que a notificação foi recebida pelo dispositivo ou aplicativo.

Este diagrama ilustra o fluxo de dados:

![diagrama wns para enviar uma notificação](images/wns-diagram-03.png)

### <a name="important-notes"></a>Observações importantes

-   O WNS não garante a confiabilidade ou a latência de uma notificação.
-   As notificações nunca devem incluir dados confidenciais ou particulares.
-   Para enviar uma notificação, o serviço na nuvem deve primeiro autenticar no WNS e receber um token de acesso.
-   Um token de acesso só permite que um serviço na nuvem envie notificações para o aplicativo para o qual o token foi criado. Um token de acesso não pode ser usado para enviar notificações entre vários aplicativos. Portanto, se o seu serviço na nuvem oferece suporte a vários apps, ele deverá fornecer o token de acesso correto para o aplicativo ao enviar uma notificação por push para cada URI de canal.
-   Quando o dispositivo está offline, o WNS armazena até cinco notificações de bloco (se a fila está habilitada; caso contrário, uma notificação de bloco) e uma notificação de selo para cada URI de canal e nenhuma notificação bruta. Esse comportamento padrão de armazenamento em cache pode ser alterado pelo [cabeçalho X-WNS-Cache-Policy](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_cache). Observe que as notificações do sistema nunca são armazenadas quando o dispositivo está offline.
-   Nos cenários onde o conteúdo da notificação é personalizado para o usuário, o WNS recomenda que o serviço na nuvem envie essas atualizações imediatamente quando elas forem recebidas. Os exemplos desse cenário incluem atualizações de feeds de mídias sociais, convites de comunicação instantânea, notificações de novas mensagens ou alertas. Como alternativa, você pode ter cenários em que a mesma atualização genérica frequentemente é fornecida a um grande subconjunto dos seus usuários. Por exemplo, atualizações de clima, cotações e notícias. As diretrizes do WNS especificam que a frequência dessas atualizações deve ser no máximo uma a cada 30 minutos. O usuário final ou o WNS pode determinar que as atualizações de rotina mais frequentes são abusivas.

## <a name="expiration-of-tile-and-badge-notifications"></a>Expiração de notificações de selo e bloco


Por padrão, as notificações de bloco e de selo expiram três dias depois que são baixadas. Quando uma notificação expira, o conteúdo é removido do bloco ou da fila e não é mais mostrado para o usuário. É recomendável definir uma expiração (usando um tempo que faça sentido para o aplicativo) em todas as notificações de bloco e de selo. Assim, você garante que o conteúdo do bloco não continue além do tempo relevante. Um tempo de expiração explícito é essencial para conteúdo com tempo de vida definido. Isso também garante a remoção de conteúdo obsoleto se seu serviço de nuvem parar de enviar notificações ou se o usuário se desconectar da rede por um período de tempo prolongado.

O serviço de nuvem pode definir uma expiração para cada notificação com a definição do cabeçalho HTTP X-WNS-TTL para especificar o tempo (em segundos) que sua notificação permanecerá válida após o envio. Para saber mais, consulte [Cabeçalhos de solicitação e resposta do serviço de notificação por push](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_ttl).

Por exemplo, durante um dia de negociação ativo do mercado de ações, você pode definir a expiração para uma atualização de preços de ações para duas vezes mais do que seu intervalo de envio (por exemplo, uma hora após o recebimento, se estiver enviando notificações a cada meia hora). Outro exemplo é um aplicativo de notícias que pode determinar que um dia é um período de expiração adequado para a atualização de blocos de notícias diárias.

## <a name="push-notifications-and-battery-saver"></a>Notificações por push e economia de bateria


A economia de bateria estende a duração da bateria, limitando a atividade em segundo plano no dispositivo. Windows 10 permite que o usuário defina a economia de bateria para ativar automaticamente quando a bateria cai abaixo de um limite especificado. Quando a economia de bateria está ativada, o recebimento de notificações por push é desabilitado para economizar energia. Mas há algumas exceções para isso. As Windows 10 configurações de economia de bateria (encontradas no aplicativo **configurações** ) permitem que seu aplicativo receba notificações por push, mesmo quando a economia de bateria está ativada.

-   **Permitir notificações por push de qualquer aplicativo em economia de bateria**: esta configuração permite que todos os aplicativos recebam notificações por push enquanto a economia de bateria está ativada. Observe que essa configuração se aplica somente ao Windows 10 para edições desktop (Home, Pro, Enterprise e Education).
-   **Sempre permitido**: esta configuração permite que aplicativos específicos sejam executados em segundo plano enquanto a economia de bateria está ativada, inclusive o recebimento de notificações por push. Essa lista é mantida manualmente pelo usuário.

Não há nenhuma maneira de se verificar o estado dessas duas configurações, mas você pode verificar o estado de economia de bateria. No Windows 10, use a propriedade [**EnergySaverStatus**](https://docs.microsoft.com/uwp/api/Windows.System.Power.PowerManager.EnergySaverStatus) para verificar o estado de economia de bateria. O aplicativo também pode usar o evento [**EnergySaverStatusChanged**](https://docs.microsoft.com/uwp/api/Windows.System.Power.PowerManager.EnergySaverStatusChanged) para escutar alterações na economia de bateria.

Se o aplicativo depende muito de notificações por push, recomendamos notificar os usuários de que eles podem não receber notificações enquanto a economia de bateria estiver ativada e facilitar para que eles possam ajustar as **configurações de economia de bateria**. Usando o esquema URI de configurações de economia de bateria no Windows 10, `ms-settings:batterysaver-settings`, você pode fornecer um link conveniente para o aplicativo configurações.

**Dica**  quando notificar o usuário sobre as configurações de economia de bateria, é recomendável fornecer uma maneira de suprimir a mensagem no futuro. Por exemplo, a caixa de seleção `dontAskMeAgainBox` no exemplo a seguir persiste a preferência do usuário em [**LocalSettings**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData.LocalSettings).

 

Aqui está um exemplo de como verificar se a economia de bateria está ativada no Windows 10. Este exemplo notifica o usuário e inicia o aplicativo Configurações para as **configurações de economia de bateria**. O `dontAskAgainSetting` permite que o usuário suprima a mensagem se ele não quiser ser notificado novamente.

```cs
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Navigation;
using Windows.System;
using Windows.System.Power;
...
...
async public void CheckForEnergySaving()
{
   //Get reminder preference from LocalSettings
   bool dontAskAgain;
   var localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
   object dontAskSetting = localSettings.Values["dontAskAgainSetting"];
   if (dontAskSetting == null)
   {  // Setting does not exist
      dontAskAgain = false;
   }
   else
   {  // Retrieve setting value
      dontAskAgain = Convert.ToBoolean(dontAskSetting);
   }
   
   // Check if battery saver is on and that it's okay to raise dialog
   if ((PowerManager.EnergySaverStatus == EnergySaverStatus.On)
         && (dontAskAgain == false))
   {
      // Check dialog results
      ContentDialogResult dialogResult = await saveEnergyDialog.ShowAsync();
      if (dialogResult == ContentDialogResult.Primary)
      {
         // Launch battery saver settings (settings are available only when a battery is present)
         await Launcher.LaunchUriAsync(new Uri("ms-settings:batterysaver-settings"));
      }

      // Save reminder preference
      if (dontAskAgainBox.IsChecked == true)
      {  // Don't raise dialog again
         localSettings.Values["dontAskAgainSetting"] = "true";
      }
   }
}
```

Este é o XAML para o [**ContentDialog**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog) apresentado neste exemplo.

```xaml
<ContentDialog x:Name="saveEnergyDialog"
               PrimaryButtonText="Open battery saver settings"
               SecondaryButtonText="Ignore"
               Title="Battery saver is on."> 
   <StackPanel>
      <TextBlock TextWrapping="WrapWholeWords">
         <LineBreak/><Run>Battery saver is on and you may 
          not receive push notifications.</Run><LineBreak/>
         <LineBreak/><Run>You can choose to allow this app to work normally
         while in battery saver, including receiving push notifications.</Run>
         <LineBreak/>
      </TextBlock>
      <CheckBox x:Name="dontAskAgainBox" Content="OK, got it."/>
   </StackPanel>
</ContentDialog>
```

## <a name="related-topics"></a>Tópicos relacionados


* [Enviar uma notificação de bloco local](sending-a-local-tile-notification.md)
* [Guia de início rápido: enviando uma notificação por push](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)
* [Como atualizar uma notificação por meio de notificações por push](https://msdn.microsoft.com/library/windows/apps/hh465450)
* [Como solicitar, criar e salvar um canal de notificação](https://msdn.microsoft.com/library/windows/apps/hh465412)
* [Como interceptar notificações para aplicativos em execução](https://msdn.microsoft.com/library/windows/apps/xaml/jj709907.aspx)
* [Como autenticar com o Serviço de Notificação por Push do Windows (WNS)](https://msdn.microsoft.com/library/windows/apps/hh465407)
* [Cabeçalhos de solicitação e resposta de serviço de notificação por push](https://msdn.microsoft.com/library/windows/apps/hh465435)
* [Diretrizes e lista de verificação de notificações por push](https://msdn.microsoft.com/library/windows/apps/hh761462)
* [Notificações brutas](https://msdn.microsoft.com/library/windows/apps/hh761488)
 

 




