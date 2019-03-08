---
Description: Programaticamente, você pode fixar o bloco principal do seu aplicativo na tela inicial, assim como é possível fixar blocos secundários. Você também pode verificar se ele está fixado no momento.
title: APIs de bloco primário
label: Primary tile API's
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp, StartScreenManager, fixar bloco primário, apis de bloco primário, verificar se o bloco está fixado, bloco dinâmico
ms.localizationpriority: medium
ms.openlocfilehash: 04d7c66b358a3a465522ad3b56d8ae926358ae57
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596191"
---
# <a name="primary-tile-apis"></a>APIs de bloco primário
 

As APIs de bloco primário permitem que você verifique se o app está fixado em Iniciar e solicite a fixação do bloco primário do app.

> [!IMPORTANT]
> **Requer a atualização para criadores**: Você deve ter como destino SDK 15063 e estar executando a build 15063 ou superior para usar as APIs de bloco principal.

> **APIs importantes**: [**Classe StartScreenManager**](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager), [ContainsAppListEntryAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_ContainsAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_), [RequestAddAppListEntryAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_RequestAddAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_)


## <a name="when-to-use-primary-tile-apis"></a>Quando usar APIs de bloco primário

Você se esforçou para criar uma ótima experiência para o bloco principal do aplicativo e agora você tem a oportunidade de solicitar ao usuário para fixá-lo em Iniciar. Antes de mergulharmos no código, aqui estão alguns itens que você deve lembrar ao projetar a experiência:

* **Crie** uma experiência do usuário sem interrupções e que possa ser ignorada facilmente no aplicativo com uma chamada de ação clara para "Fixar Bloco Dinâmico".
* **Explique** claramente o valor do Bloco Dinâmico do aplicativo antes de pedir ao usuário para fixá-lo.
* **Não** peça a um usuário para fixar o bloco do aplicativo se já tiver sido fixado ou se o dispositivo não oferecer suporte a ele (mais informações a seguir).
* **Não** peça repetidamente para o usuário fixar o bloco do aplicativo (eles provavelmente ficarão aborrecidos).
* **Não** chame a fixação de API sem interação explícita do usuário ou quando o app estiver minimizado/não abrir.


## <a name="checking-whether-the-apis-exist"></a>Verificar se as APIs existem

Se o aplicativo oferece suporte às versões mais antigas do Windows 10, é preciso verificar se essas APIs de bloco primário estão disponíveis. É possível fazer isso usando ApiInformation. Se as APIs do bloco principal não estiverem disponíveis, evitar a execução de todas as chamadas para as APIs.

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.StartScreen.StartScreenManager"))
{
    // Primary tile API's supported!
}
else
{
    // Older version of Windows, no primary tile API's
}
```


## <a name="check-if-start-supports-your-app"></a>Verifique se Iniciar oferece suporte ao aplicativo

Dependendo do menu Iniciar atual e do tipo de aplicativo, pode não existir suporte para a fixação do aplicativo na tela inicial atual. Somente computador desktop e dispositivos móveis oferecem suporte à fixação do bloco primário em Iniciar. Portanto, antes de mostrar qualquer interface do usuário de fixação ou executar qualquer código, primeiro você precisa verificar se o aplicativo é suportado na tela inicial atual. Caso não exista o suporte, não solicite ao usuário para fixar o bloco.

```csharp
// Get your own app list entry
// (which is always the first app list entry assuming you are not a multi-app package)
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// Check if Start supports your app
bool isSupported = StartScreenManager.GetDefault().SupportsAppListEntry(entry);
```


## <a name="check-whether-youre-currently-pinned"></a>Verifique se você está atualmente fixado

Para descobrir se o bloco primário está atualmente fixado em Iniciar, use o método [ContainsAppListEntryAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_ContainsAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_).

```csharp
// Get your own app list entry
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// Check if your app is currently pinned
bool isPinned = await StartScreenManager.GetDefault().ContainsAppListEntryAsync(entry);
```


##  <a name="pin-your-primary-tile"></a>Fixar o bloco primário

Se o bloco primário atualmente não estiver fixado e o bloco for compatível com Iniciar, convém mostrar uma dica para que os usuários possam fixar o bloco primário.

> [!NOTE]
> Você deve chamar essa API de um thread de interface do usuário enquanto seu aplicativo estiver em primeiro plano e você só deve chamar essa API depois que o usuário solicitou intencionalmente o bloco primário ser fixado (por exemplo, depois que o usuário clicou em Sim para sua dica sobre como fixar o bloco).

Se o usuário clica no botão para fixar o bloco primário, você poderia chamar o método [RequestAddAppListEntryAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_RequestAddAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_) para solicitar que o bloco seja fixado em Iniciar. Isso exibirá uma caixa de diálogo solicitando ao usuário para confirmar se eles querem seu bloco fixado em Iniciar.

Isso retornará um valor booleano que representa se o bloco agora está fixado em Iniciar. Se o bloco já foi fixado, isso retornará imediatamente o valor true sem mostrar a caixa de diálogo para o usuário. Se o usuário clicar em não na caixa de diálogo ou se não existir suporte para a fixação do bloco em Iniciar, isso retornará false. Caso contrário, o usuário clicou em sim e o bloco foi fixado, e a API retornará true.

```csharp
// Get your own app list entry
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// And pin it to Start
bool isPinned = await StartScreenManager.GetDefault().RequestAddAppListEntryAsync(entry);
```


## <a name="resources"></a>Recursos

* [Exemplo de código completo no GitHub](https://github.com/WindowsNotifications/quickstart-pin-primary-tile)
* [Fixar na barra de tarefas](../pin-to-taskbar.md)
* [Blocos, selos e notificações](index.md)
* [Documentação do bloco adaptável](create-adaptive-tiles.md)
