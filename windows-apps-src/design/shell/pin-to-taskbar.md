---
author: mijacobs
Description: You can programmatically pin your app to the taskbar,  bnd you can check if it's currently pinned.
title: Fixar seu aplicativo na barra de tarefas
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, barra de tarefas, gerenciador de barra de tarefas, fixar na barra de tarefas, bloco principal
ms.localizationpriority: medium
ms.openlocfilehash: 47fcd1f9d090c49ecbd49e05696b33f789973160
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6986767"
---
# <a name="pin-your-app-to-the-taskbar"></a>Fixar seu aplicativo na barra de tarefas

Programaticamente, você pode fixar o bloco principal do seu app na barra de tarefa, da mesma forma que pode [fixar seu app no menu Iniciar](tiles-and-notifications/primary-tile-apis.md). E você pode verificar se seu app está fixado e se a barra de tarefas permite que se fixe apps. 

![Barra de tarefas](images/taskbar/taskbar.png)

> [!IMPORTANT]
> **Requer a Fall Creators Update**: você precisa usar o SDK 16299 e executar a compilação 16299 ou mais recente para usar as APIs de barra de tarefa.

> **APIs importantes**: [classe TaskbarManager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager) 


## <a name="when-should-you-ask-the-user-to-pin-your-app-to-the-taskbar"></a>Quando você deve pedir ao usuário para fixar seu app na barra de tarefas? 

A [classe TaskbarManager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager) permite que você peça ao usuário para fixar seu app na barra de tarefas; o usuário deverá aprovar a solicitação. Você se esforçou para criar um app fora de série e agora você tem a oportunidade de pedir ao usuário para fixá-lo na barra de tarefas. Antes de mergulharmos no código, aqui estão alguns itens que você deve lembrar ao criar sua experiência:

* **Crie** uma experiência do usuário sem interrupções e que não possa ser ignorada facilmente no app com uma chamada de ação clara para "Fixar na barra de tarefas". Evite usar caixas de diálogo e submenus para essa finalidade. 
* **Explique** claramente o valor de seu app antes de pedir ao usuário para fixá-lo na barra de tarefas.
* **Não** peça a um usuário para fixar o app se o bloco já tiver sido fixado ou se o dispositivo não oferecer suporte a isso. (Este artigo explica como determinar se há suporte para a fixação.)
* **Não** peça repetidamente para o usuário fixar seu app (eles provavelmente ficarão aborrecidos).
* **Não** chame a fixação de API sem interação explícita do usuário ou quando o app estiver minimizado/não abrir.


## <a name="1-check-whether-the-required-apis-exist"></a>1. Verifique se as APIs necessárias existem

Se o aplicativo oferece suporte às versões mais antigas do Windows 10, é preciso verificar se a classe TaskbarManager está disponível. Você pode usar o [método ApiInformation.IsTypePresent](https://docs.microsoft.com/en-us/uwp/api/windows.foundation.metadata.apiinformation#Windows_Foundation_Metadata_ApiInformation_IsTypePresent_System_String_) para executar essa verificação. Se a classe TaskbarManager não estiver disponível, evite a execução de chamadas às APIs.

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.Shell.TaskbarManager"))
{
    // Taskbar APIs exist!
}

else
{
    // Older version of Windows, no taskbar APIs
}
```


## <a name="2-check-whether-taskbar-is-present-and-allows-pinning"></a>2. Verifique se a barra de tarefas está presente e permite a fixação

Os aplicativos UWP podem ser executados em uma ampla variedade de dispositivos; nem todas eles são compatíveis com a barra de tarefas. No momento, somente dispositivos desktop são compatíveis com a barra de tarefas. 

Mesmo que a barra de tarefas esteja disponível, uma política de grupo no computador do usuário pode desabilitar a fixação na barra de tarefas. Portanto, antes de tentar fixar seu app, você precisa verificar se a opção é possível fixar apps na barra de tarefas. A [propriedade TaskbarManager.IsPinningAllowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsPinningAllowed) retorna true se a barra de tarefas estiver presente e permitir a fixação. 

```csharp
// Check if taskbar allows pinning (Group Policy can disable it, or some device families don't have taskbar)
bool isPinningAllowed = TaskbarManager.GetDefault().IsPinningAllowed;
```

> [!NOTE]
> Se você não quiser fixar seu app na barra de tarefas e apenas quiser saber se a barra de tarefas está disponível, use a [propriedade TaskbarManager.IsSupported](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsSupported).


## <a name="3-check-whether-your-app-is-currently-pinned-to-the-taskbar"></a>3. Verifique se o seu app está atualmente fixado na barra de tarefas

Obviamente, não faz sentido pedir ao usuário para permitir que você fixe o app na barra de tarefas se ele já estiver fixado lá. Você pode usar o [método TaskbarManager.IsCurrentAppPinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsCurrentAppPinnedAsync) para verificar se o app já está fixado antes de pedir que o usuário.

```csharp
// Check whether your app is currently pinned
bool isPinned = await TaskbarManager.GetDefault().IsCurrentAppPinnedAsync();

if (isPinned)
{
    // The app is already pinned--no point in asking to pin it again!
}
else 
{
    //The app is not pinned. 
}
```


##  <a name="4-pin-your-app"></a>4. Fixe seu app

Se a barra de tarefas estiver presente e a fixação for permitida e seu app não estiver fixado, você talvez queira mostrar uma dica sutil para que os usuários saibam que podem fixar seu app. Por exemplo, você pode mostrar um ícone de fixação em algum lugar na interface do usuário no qual o usuário poderá clicar. 

Se o usuário clicar na interface do usuário de sugestão de fixação, você poderá chamar o [método TaskbarManager.RequestPinCurrentAppAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.RequestPinCurrentAppAsync). Esse método exibirá uma caixa de diálogo que pede ao usuário para confirmar se ele quer seu app fixado na barra de tarefas.

> [!IMPORTANT]
> Isso deve ser chamado direto de um thread de interface do usuário em primeiro plano, caso contrário, uma exceção será lançada.

```csharp
// Request to be pinned to the taskbar
bool isPinned = await TaskbarManager.GetDefault().RequestPinCurrentAppAsync();
```

![Caixa de diálogo de fixação](images/taskbar/pin-dialog.png)

Esse método retorna um valor booliano que indica se o seu app está fixado na barra de tarefas. Se o app já foi fixado, o método retornará imediatamente o valor true sem mostrar a caixa de diálogo para o usuário. Se o usuário clicar em "não" na caixa de diálogo ou se não for permitido fixar o app na barra de tarefas, o método retornará false. Caso contrário, ou seja, o usuário tiver clicado em "sim" e o app estiver fixado, a API retornará true.


## <a name="resources"></a>Recursos

* [Exemplo de código completo em GitHub](https://github.com/WindowsNotifications/quickstart-pin-to-taskbar)
* [Classe TaskbarManager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [Fixar um app no menu Iniciar](tiles-and-notifications/primary-tile-apis.md)