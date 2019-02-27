---
title: Iniciar a captura de tela
description: Este tópico descreve os esquemas de URI ms-screenclip e ms-screensketch. Seu aplicativo pode usar esses esquemas de URI para iniciar o aplicativo de esboço & recorte ou abrir um novo recorte.
ms.date: 08/09/2017
ms.topic: article
keywords: Windows 10, uwp, uri, recorte, esboço
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 06e988387f574b74d511b14a2ebca24b0a149158
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9116168"
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
| callbackformat | string | não | Este parâmetro não está disponível. |

## <a name="launching-the-snip--sketch-app"></a>Iniciar o aplicativo de esboço & recorte

O **ms-screensketch:** URI permite que você iniciar o aplicativo de esboço & recorte programaticamente e abrir uma imagem específica no aplicativo para anotação.

**ms-screensketch:** usa os seguintes parâmetros:

| Parâmetro | Tipo | Obrigatório | Descrição |
| --- | --- | --- | --- |
| sharedAccessToken | string | não | Um token que identifica o arquivo para abrir no aplicativo recorte & esboço. Recuperou do [SharedStorageAccessManager.AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile). Se esse parâmetro é omitido, o aplicativo será iniciado sem abrir um arquivo. |
| secondarySharedAccessToken | string | não | Uma cadeia de caracteres que identifica um arquivo JSON com metadados sobre o recorte. Os metadados podem incluir um campo **clipPoints** com uma matriz de coordenadas x, y e/ou um [userActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity). |
| fonte | string | não | Uma sequência de forma livre para indicar a origem que iniciou o URI. |
| isTemporary | bool | não | Se definido como verdadeiro, esboço da tela tentará excluir o arquivo após abri-lo. |

O exemplo a seguir chama o método [LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) para enviar uma imagem para recorte & Esboço do aplicativo do usuário.

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```

O exemplo a seguir ilustra o que pode conter um arquivo especificado pelo parâmetro **secondarySharedAccessToken** de **ms-screensketch** :

```json
{
  "clipPoints": [
    {
      "x": 0,
      "y": 0
    },
    {
      "x": 2080,
      "y": 0
    },
    {
      "x": 2080,
      "y": 780
    },
    {
      "x": 0,
      "y": 780
    }
  ],
  "userActivity": "{\"$schema\":\"http://activity.windows.com/user-activity.json\",\"UserActivity\":\"type\",\"1.0\":\"version\",\"cross-platform-identifiers\":[{\"platform\":\"windows_universal\",\"application\":\"Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge\"},{\"platform\":\"host\",\"application\":\"edge.activity.windows.com\"}],\"activationUrl\":\"microsoft-edge:https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"contentUrl\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"visualElements\":{\"attribution\":{\"iconUrl\":\"https://www.microsoft.com/favicon.ico?v2\",\"alternateText\":\"microsoft.com\"},\"description\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"backgroundColor\":\"#FF0078D7\",\"displayText\":\"Use snipping tool to capture screenshots - Windows Help\",\"content\":{\"$schema\":\"http://adaptivecards.io/schemas/adaptive-card.json\",\"type\":\"AdaptiveCard\",\"version\":\"1.0\",\"body\":[{\"type\":\"Container\",\"items\":[{\"type\":\"TextBlock\",\"text\":\"Use snipping tool to capture screenshots - Windows Help\",\"weight\":\"bolder\",\"size\":\"large\",\"wrap\":true,\"maxLines\":3},{\"type\":\"TextBlock\",\"text\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"size\":\"normal\",\"wrap\":true,\"maxLines\":3}]}]}},\"isRoamable\":true,\"appActivityId\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\"}"
}

```
