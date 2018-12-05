---
title: Iniciar a captura de tela
description: Este tópico descreve os esquemas de URI ms-screenclip e ms-screensketch. Seu aplicativo pode usar esses esquemas de URI para iniciar o aplicativo de recorte & esboço ou abrir um novo recorte.
ms.date: 8/1/2017
ms.topic: article
keywords: Windows 10, uwp, uri, recorte, esboço
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 7aa0b70aee50c79088a68378fa75664711c3d564
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8705535"
---
# <a name="launch-screen-snipping"></a>Iniciar a captura de tela

O **ms-screenclip:** e **ms-screensketch:** esquemas URI permite que você inicie o recorte ou editar capturas de tela.

## <a name="open-a-new-snip-from-your-app"></a>Abra um novo recorte do seu aplicativo

O **ms-screenclip:** URI permite que seu aplicativo abrir automaticamente e iniciar um novo recorte. O recorte resultante é copiado para a área de transferência do usuário, mas não será automaticamente passado para o aplicativo de abertura.

**ms-screenclip:** usa os seguintes parâmetros:

| Parâmetro | Tipo | Obrigatório | Descrição |
| --- | --- | --- | --- |
| fonte | string | não | Uma sequência de forma livre para indicar a origem que iniciou o URI. |
| delayInSeconds | int | não | Um valor inteiro, de 1 a 30. Especifica o atraso, em segundos completos, entre a chamada URI e recorte começa. |

## <a name="launching-the-snip--sketch-app"></a>Iniciar o aplicativo de esboço & recorte

O **ms-screensketch:** URI permite que você iniciar o aplicativo de recorte & esboço programaticamente e abrir uma imagem específica no aplicativo para anotação.

**ms-screensketch:** usa os seguintes parâmetros:

| Parâmetro | Tipo | Obrigatório | Descrição |
| --- | --- | --- | --- |
| sharedAccessToken | string | não | Um token que identifica o arquivo para abrir no aplicativo recorte & esboço. Recuperou do [SharedStorageAccessManager.AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile). Se esse parâmetro é omitido, o aplicativo será iniciado sem abrir um arquivo. |
| fonte | string | não | Uma sequência de forma livre para indicar a origem que iniciou o URI. |
| isTemporary | bool | não | Se definido como verdadeiro, esboço da tela tentará excluir o arquivo após abri-lo. |

O exemplo a seguir chama o método [LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) para enviar uma imagem para recorte & Esboço do aplicativo do usuário.

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```
