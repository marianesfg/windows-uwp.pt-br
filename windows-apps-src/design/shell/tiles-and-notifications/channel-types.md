---
author: adwilso
Description: Windows Push Notification Services (WNS) enables third-party developers to send toast, tile, badge, and raw updates from their own cloud service. There are many ways to send the notifications depending on the needs of your application
title: Escolhendo o tipo de canal de notificação por push certo
ms.author: mijacobs
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 88040d6eb1c5d198de0a446348e114b7e59a2dad
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654895"
---
# <a name="choosing-the-right-push-notification-channel-type"></a>Escolhendo o tipo de canal de notificação por push certo

Este artigo trata dos três tipos de canais de notificação por push UWP (primários, secundários e alternativos) que ajudam você a fornecer conteúdo no seu aplicativo. 

(Para saber mais sobre a criação de notificações por push, veja a [Visão geral dos Serviços de Notificação por Push do Windows (WNS)](../tiles-and-notifications/windows-push-notification-services--wns--overview.md).) 

## <a name="types-of-push-channels"></a>Tipos de canais de push 

Há três tipos de canais de push que podem ser usados para enviar notificações para um aplicativo UWP. São eles: 

[Canal primário](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) - o canal de push "tradicional". Pode ser usado por qualquer aplicativo na loja para enviar notificações do sistema, de blocos, de selos ou brutas (Link para descrições de notificações do sistema/blocos/selo)

[Canal de bloco secundário](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) - usado para enviar atualizações de bloco para um bloco secundário. Só pode ser usado para enviar notificações de bloco ou selo para um bloco secundário fixado na tela inicial do usuário

[Canal alternativo](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) - um novo tipo de canal adicionado na Atualização para Criadores. Ele permite o envio de notificações brutas a qualquer aplicativo UWP, incluindo aqueles que não estão registrados na Store. 

> [!NOTE]
> Não importa qual canal de push você usar, quando o aplicativo estiver em execução no dispositivo, ele sempre será capaz de enviar notificações do sistema local, de bloco ou de selo. Ele pode enviar notificações locais dos processos de aplicativo em primeiro plano ou de uma tarefa em segundo plano. 


## <a name="primary-channels"></a>Canais primários

Esses são os canais mais usados no Windows atualmente e eles são úteis em praticamente qualquer cenário de distribuição do seu aplicativo por meio da Microsoft Store. Eles permitem que você envie todos os tipos de notificação para o aplicativo. 

### <a name="what-do-primary-channels-enable"></a>O que os canais primários permitem?

-   **Enviar atualizações de bloco ou selo ao bloco primário.** Se o usuário tiver optado por fixar seu bloco na tela inicial, essa é a sua chance de mostrar. Enviar atualizações com informações ou lembretes úteis de experiências dentro de seu aplicativo. 
-   **Enviar notificações do sistema.** As notificações do sistema são uma oportunidade de apresentar informações ao usuário imediatamente. Elas são pintadas pelo shell sobre a maioria dos aplicativos e residem na central de ações para que o usuário possa voltar e interagir com elas posteriormente. 
-   **Enviar notificações brutas para acionar uma tarefa em segundo plano.** Às vezes você deseja fazer algum trabalho em nome do usuário com base em uma notificação. As notificações brutas permitem que as tarefas em segundo plano do seu aplicativo sejam executadas 
-   **Criptografia de mensagem em trânsito fornecida pelo Windows usando o TLS.** As mensagens são criptografadas durante a transmissão tanto na chegada ao WNS como na ida para o dispositivo do usuário.  

### <a name="limitations-of-primary-channels"></a>Limitações de canais primários

-   Requerem o uso de API REST do WNS para enviar notificações, o que não é o padrão entre os fornecedores de dispositivos. 
-   Apenas um canal pode ser criado por aplicativo 
-   Requerem que o aplicativo esteja registrado na Microsoft Store

### <a name="creating-a-primary-channel"></a>Criando um canal primário 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
```

## <a name="secondary-tile-channels"></a>Canais de bloco secundário

Esses canais podem ser usados para enviar atualizações de bloco e selo para um bloco secundário. Eles são usados por aplicativos para notificar os usuários sobre ações ou informações interessantes com as quais eles podem interagir no aplicativo, como novas mensagens em um chat em grupo ou um placar de esportes atualizado. 

### <a name="what-do-secondary-tile-channels-enable"></a>O que os canais de bloco secundário permitem?

-   Enviar notificações de bloco ou selo para blocos secundários. Blocos secundários são uma ótima maneira de trazer usuários de volta ao seu aplicativo. Eles constituem uma conexão profunda com as informações que importam para eles, e colocar informações relevantes nos blocos ajuda a trazê-los de volta repetidamente.
-   Separação de canais (e expirações) entre vários blocos. Isso permite separar a lógica no back-end entre os vários tipos de blocos secundários que um usuário pode fixar na tela inicial. 
-   Criptografia de mensagem em trânsito fornecida pelo Windows usando o TLS. As mensagens são criptografadas durante a transmissão tanto na chegada ao WNS como na ida para o dispositivo do usuário.  

### <a name="limitations-of-secondary-tile-channels"></a>Limitações de canais de bloco secundário

-   Notificações do sistema e notificações brutas não são permitidas. Notificações do sistema ou notificações brutas enviadas para um bloco secundário são ignoradas pelo sistema.
-   Requerem que o aplicativo esteja registrado na Microsoft Store


### <a name="creating-a-secondary-tile-channel"></a>Criando um canal de bloco secundário 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForSecondaryTileAsync(tileId);
```

## <a name="alternate-channels"></a>Canais alternativos

Os canais alternativos permitem que os aplicativos enviem notificações por push sem que haja a necessidade de registro na Microsoft Store ou da criação de canais de push fora do canal primário usado para o aplicativo. 
 
### <a name="what-do-alternate-channels-enable"></a>O que os canais alternativos permitem?
-   Enviar notificações brutas por push para um UWP em execução em qualquer dispositivo Windows. Canais alternativos permitem apenas notificações brutas.
-   Permitem que aplicativos criem vários canais de push brutos para diferentes recursos dentro do aplicativo. Um aplicativo pode criar até 1.000 canais alternativos, e cada um é válido por 30 dias. Cada um desses canais pode ser gerenciado ou revogado separadamente pelo aplicativo.
-   Os canais de push alternativos podem ser criados sem registrar um aplicativo na Microsoft Store. Se o seu aplicativo for instalado em dispositivos sem que tenha sido registrado na Microsoft Store, ele ainda poderá receber notificações por push.
-   Servidores podem enviar notificações usando protocolos APIs REST padrão W3C e VAPID. Canais alternativos usam o protocolo padrão W3C, isso permite que você simplifique a lógica do servidor que precisa ser mantida.
-   Criptografia de mensagem completa, de ponta a ponta. O canal primário fornece criptografia em trânsito, mas se você quiser mais segurança, canais alternativos permitem que seu aplicativo passe por cabeçalhos de criptografia para proteger uma mensagem. 

### <a name="limitations-of-alternate-channels"></a>Limitações de canais alternativos
-   Os aplicativos não podem enviar notificações do sistema, de bloco ou de selo. O canal alternativo limita sua capacidade de enviar outros tipos de notificação. O aplicativo ainda é capaz de enviar notificações locais a partir da tarefa em segundo plano. 
-   Requerem uma API REST diferente dos canais de bloco primário ou secundário. Usar a API REST padrão W3C significa que o aplicativo precisará ter uma lógica diferente para enviar atualizações de notificações do sistema ou de bloco

### <a name="creating-an-alternate-channel"></a>Criando um canal alternativo 

```csharp
PushNotificationChannel webChannel = 
    await PushNotificationChannelManager.Current.CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId);
```

## <a name="channel-type-comparison"></a>Comparação de tipos de canal
Aqui está uma rápida comparação entre os diferentes tipos de canal:

<table>

<tr class="header">
<th align="left"><b>Tipo</b></th>
<th align="left"><b>Envia notificação do sistema?</b></th>
<th align="left"><b>Envia notificações de bloco/selo?</b></th>
<th align="left"><b>Envia notificações brutas?</b></th>
<th align="left"><b>Autenticação</b></th>
<th align="left"><b>API</b></th>
<th align="left"><b>É necessário o registro na Store?</b></th>
<th align="left"><b>Canais</b></th>
<th align="left"><b>Criptografia</b></th>
</tr>


<tr class="odd">
<td align="left">Primário</td>
<td align="left">Sim</td>
<td align="left">Sim - somente bloco primário</td>
<td align="left">Sim</td>
<td align="left">OAuth</td>
<td align="left">API REST DO WNS</td>
<td align="left">Sim</td>
<td align="left">Um por aplicativo</td>
<td align="left">Em trânsito</td>
</tr>
<tr class="even">
<td align="left">Bloco Secundário</td>
<td align="left">Não</td>
<td align="left">Sim - somente bloco secundário</td>
<td align="left">Não</td>
<td align="left">OAuth</td>
<td align="left">API REST DO WNS</td>
<td align="left">Sim</td>
<td align="left">Um por bloco secundário</td>
<td align="left">Em trânsito</td>
</tr>
<tr class="odd">
<td align="left">Alternativo</td>
<td align="left">Não</td>
<td align="left">Não</td>
<td align="left">Sim</td>
<td align="left">VAPID</td>
<td align="left">Padrão W3C de WebPush</td>
<td align="left">Não</td>
<td align="left">1.000 por aplicativo</td>
<td align="left">Criptografia em trânsito + criptografia de ponta a ponta possíveis com passagem de cabeçalho (requer o código do aplicativo)</td>
</tr>
</table>

## <a name="choosing-the-right-channel"></a>Escolhendo o canal certo

Em geral, recomendamos que você use o canal primário no aplicativo, com algumas exceções: 

1. Se você estiver enviando uma atualização de bloco para um bloco secundário, use o canal de push do bloco secundário.
2. Se você estiver distribuindo canais para outros serviços (como no caso de um navegador), use o canal alternativo.
3. Se você estiver criando um aplicativo que não será listado na Microsoft Store (por exemplo, um aplicativo LOB), use um canal alternativo.
4. Se você tiver um código de push da Web existente em seu servidor que você deseja reutilizar ou, se precisar de vários canais em seu serviço de back-end, use canais alternativos.

## <a name="related-articles"></a>Artigos relacionados

* [Enviar uma notificação de bloco local](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [Notificações do sistema interativas e adaptáveis](../tiles-and-notifications/adaptive-interactive-toasts.md)
* [Guia de início rápido: enviando uma notificação por push](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)
* [Como atualizar uma notificação por meio de notificações por push](https://msdn.microsoft.com/library/windows/apps/hh465450)
* [Como solicitar, criar e salvar um canal de notificação](https://msdn.microsoft.com/library/windows/apps/hh465412)
* [Como interceptar notificações para aplicativos em execução](https://msdn.microsoft.com/library/windows/apps/hh465450)
* [Como autenticar com o Serviço de Notificação por Push do Windows (WNS)](https://msdn.microsoft.com/library/windows/apps/hh465407)
* [Cabeçalhos de solicitação e resposta de serviço de notificação por push](https://msdn.microsoft.com/library/windows/apps/hh465435)
* [Diretrizes e lista de verificação de notificações por push](https://msdn.microsoft.com/library/windows/apps/hh761462)
* [Notificações brutas](raw-notification-overview.md)
