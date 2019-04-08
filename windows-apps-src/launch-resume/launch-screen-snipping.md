---
title: Iniciar a captura de tela
description: Este tópico descreve os esquemas URI screenclip ms e ms-screensketch. Seu aplicativo pode usar esses esquemas URI para iniciar o aplicativo de recorte & esboço ou para abrir uma nova captura.
ms.date: 08/09/2017
ms.topic: article
keywords: Windows 10, uwp, uri, recorte e esboço
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 06e988387f574b74d511b14a2ebca24b0a149158
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595381"
---
# <a name="launch-screen-snipping"></a>Iniciar a captura de tela

O **screenclip ms:** e **screensketch ms:** Esquemas de URI permite que você iniciar o recorte ou capturas de tela de edição.

## <a name="open-a-new-snip-from-your-app"></a>Abra uma nova captura do seu aplicativo

O **screenclip ms:** URI permite que seu aplicativo abrirá automaticamente e iniciar uma nova captura. O recorte resultante é copiado para a área de transferência do usuário, mas não será automaticamente passado para o aplicativo de abertura.

**MS-screenclip:** usa os seguintes parâmetros:

| Parâmetro | Tipo | Obrigatório | Descrição |
| --- | --- | --- | --- |
| Código-fonte | cadeia de caracteres | não | Uma cadeia de forma livre para indicar a origem que iniciou o URI. |
| delayInSeconds | int | não | Um valor de inteiro, de 1 a 30. Especifica o atraso em segundos totais, entre a chamada URI e recorte começa. |
| callbackformat | cadeia de caracteres | não | Esse parâmetro não está disponível. |

## <a name="launching-the-snip--sketch-app"></a>Iniciando o aplicativo de esboço e recorte

O **screensketch ms:** URI permite que você iniciar o aplicativo de recorte & esboço programaticamente e abrir uma imagem específica em que o aplicativo para a anotação.

**MS-screensketch:** usa os seguintes parâmetros:

| Parâmetro | Tipo | Obrigatório | Descrição |
| --- | --- | --- | --- |
| sharedAccessToken | cadeia de caracteres | não | Um token de identificação de arquivo a ser aberto no aplicativo do esboço & de recorte. Recuperados [SharedStorageAccessManager.AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile). Se esse parâmetro for omitido, o aplicativo será iniciado sem um arquivo aberto. |
| secondarySharedAccessToken | cadeia de caracteres | não | Uma cadeia de caracteres que identifica um arquivo JSON com metadados sobre o recorte. Os metadados podem incluir um **clipPoints** campo com uma matriz de coordenadas x, y, e/ou um [userActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity). |
| Código-fonte | cadeia de caracteres | não | Uma cadeia de forma livre para indicar a origem que iniciou o URI. |
| IsTemporary | bool | não | Se definido como True, a tela de esboço tentará excluir o arquivo depois de abri-lo. |

A exemplo a seguir chama o [LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) método para enviar uma imagem para recorte & Esboço do aplicativo do usuário.

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```

O exemplo a seguir ilustra o que um arquivo especificado pela **secondarySharedAccessToken** parâmetro do **ms screensketch** pode conter:

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
