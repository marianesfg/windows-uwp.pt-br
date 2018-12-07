---
Description: Learn how to use custom audio on your toast notifications.
title: Áudio personalizado em notificações do sistema
label: Custom audio on toasts
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: windows 10, uwp, notificação do sistema, áudio personalizado, notificação, áudio, som
ms.localizationpriority: medium
ms.openlocfilehash: 982340901d13f17945c1e7ffa11099f52732f619
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8780733"
---
# <a name="custom-audio-on-toasts"></a>Áudio personalizado em notificações do sistema

As notificações do sistema podem usar áudio personalizado, o que permite ao aplicativo expressar os efeitos de som exclusivos da sua marca. Por exemplo, um aplicativo de mensagens pode usar seus próprios sons nas mensagens nas notificações do sistema para que o usuário saiba instantaneamente que eles receberam uma notificação do aplicativo, em vez de ouvir o som de notificação genérico.

## <a name="install-uwp-community-toolkit-nuget-package"></a>Instalar o pacote NuGet do kit de ferramentas da comunidade UWP

Para criar notificações através do código, é altamente recomendável usar a biblioteca de Notificações do kit de ferramentas da comunidade UWP, que fornece um modelo de objeto para a conteúdo XML da notificação. Você pode construir manualmente o XML de notificação, mas isso é propenso a erros e confuso. A biblioteca de notificações no Kit de ferramentas da comunidade UWP é criado e mantido pela equipe especialista em notificações na Microsoft.

Instale [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) de NuGet (estamos usando a versão 1.0.0 nesta documentação).


## <a name="add-namespace-declarations"></a>Adicionar declarações de namespace

`Windows.UI.Notifications` inclui a API de bloco e notificação do sistema. `Microsoft.Toolkit.Uwp.Notifications` inclui a biblioteca de Notificações.

```csharp
using Microsoft.Toolkit.Uwp.Notifications;
using Windows.UI.Notifications;
```


## <a name="construct-the-notification"></a>Construir a notificação

O conteúdo da notificação do sistema inclui texto e imagens, além de botões e entradas. Consulte [enviar notificação do sistema local](send-local-toast.md) para ver um trecho do código completo.

```csharp
ToastContent toastContent = new ToastContent()
{
    Visual = new ToastVisual()
    {
        ... (omitted)
    }
};
```


## <a name="add-the-custom-audio"></a>Adicionar áudio personalizado

O Windows Mobile sempre ofereceu suporte a áudio personalizado nas notificações do sistema. No entanto, a área de trabalho adicionou somente o suporte para áudio personalizado na Versão 1511 (build 10586). Se você enviar uma notificação do sistema com o áudio personalizados para um dispositivo da área de trabalho antes de versão 1511, a notificação do sistema ficará silenciosa. Portanto, para a versão de pré-lançamento da área de trabalho 1511, você não deve incluir o áudio personalizado em sua notificação do sistema, pois a notificação usará pelo menos o som de notificação padrão.

**Problema conhecido**: se você estiver usando a área de trabalho versão 1511, o áudio de notificação do sistema personalizado funcionará somente se o aplicativo for instalado pela Store. Isso significa que você não pode testar localmente seu áudio personalizados na área de trabalho antes de enviá-lo para a Store, mas o áudio funcionará quando instalado pela Store. Corrigimos isso na Atualização de Aniversário, para que o áudio personalizado do seu aplicativo implantado localmente funcione corretamente.

```csharp
?
bool supportsCustomAudio = true;
 
// If we're running on Desktop before Version 1511, do NOT include custom audio
// since it was not supported until Version 1511, and would result in a silent toast.
if (AnalyticsInfo.VersionInfo.DeviceFamily.Equals("Windows.Desktop")
    && !ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 2))
{
    supportsCustomAudio = false;
}
 
if (supportsCustomAudio)
{
    toastContent.Audio = new ToastAudio()
    {
        Src = new Uri("ms-appx:///Assets/Audio/CustomToastAudio.m4a")
    };
}
```

Os tipos de arquivo com suporte incluem...

- .aac
- .flac
- .m4a
- .mp3
- .wav
- .wma


## <a name="send-the-notification"></a>Enviar a notificação

Agora que seu conteúdo de notificação do sistema está completo, é fácil enviar uma notificação.

```csharp
// Create the toast notification from the previous toast content
ToastNotification notification = new ToastNotification(toastContent.GetXml());
             
// And then send the toast
ToastNotificationManager.CreateToastNotifier().Show(notification);
```


## <a name="related-topics"></a>Tópicos relacionados

- [Exemplo de código completo em GitHub](https://github.com/WindowsNotifications/quickstart-toast-with-custom-audio)
- [Enviar uma notificação do sistema local](send-local-toast.md)
- [Conteúdo e documentação sobre notificações do sistema](adaptive-interactive-toasts.md)