---
author: QuinnRadich
title: Iniciar a captura de tela
description: Este tópico descreve os esquemas de URI ms-screenclip e ms-screensketch. Seu aplicativo pode usar esses esquemas de URI para iniciar o aplicativo de recorte e esboço ou abrir uma nova captura.
ms.author: quradic
ms.date: 8/1/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, uri, recorte, esboço
ms.localizationpriority: medium
ms.openlocfilehash: e18662125ef72051a289b3f1d0f3dc09b452d256
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4502372"
---
# <a name="launch-screen-snipping"></a>Iniciar a captura de tela

O **ms-screenclip:** e **ms-screensketch:** esquemas de URI permite que você inicie o recorte ou editar capturas de tela.

## <a name="open-a-new-snip-from-your-app"></a>Abrir uma nova captura do seu aplicativo

O **ms-screenclip:** URI permite que seu aplicativo abrir e iniciar uma nova captura automaticamente. O recorte resultante é copiado para área de transferência do usuário, mas não é automaticamente passado para o aplicativo de abertura.

**ms-screenclip:** usa os seguintes parâmetros:

| Parâmetro | Tipo | Obrigatório | Descrição |
| --- | --- | --- | --- |
| fonte | string | não | Uma cadeia de caracteres forma livre para indicar a origem que iniciou o URI. |
| delayInSeconds | int | não | Um valor inteiro, de 1 a 30. Especifica o atraso, em segundos completos, entre a chamada URI e recorte começa. |

## <a name="launching-the-snip--sketch-app"></a>Iniciar o aplicativo de esboço e recorte

O **ms-screensketch:** URI permite que você iniciar o aplicativo de recorte e esboço programaticamente e abrir uma imagem específica no aplicativo para anotação.

**ms-screensketch:** usa os seguintes parâmetros:

| Parâmetro | Tipo | Obrigatório | Descrição |
| --- | --- | --- | --- |
| sharedAccessToken | string | não | Um token que identifica o arquivo para abrir no aplicativo recorte e esboço. Obtida [SharedStorageAccessManager.AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile). Se esse parâmetro é omitido, o aplicativo será iniciado sem abrir um arquivo. |
| fonte | string | não | Uma cadeia de caracteres forma livre para indicar a origem que iniciou o URI. |
| isTemporary | bool | não | Se definido como verdadeiro, esboço da tela tentará excluir o arquivo após abri-lo. |

O exemplo a seguir chama o método [LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) para enviar uma imagem para recorte e Esboço do aplicativo do usuário.

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```