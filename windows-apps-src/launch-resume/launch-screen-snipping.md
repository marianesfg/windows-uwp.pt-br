---
title: Iniciar a captura de tela
description: Este tópico descreve os esquemas de URI MS-screenclip e MS-screensketch. Seu aplicativo pode usar esses esquemas de URI para iniciar o aplicativo recorte & esboço ou para abrir um novo recorte.
ms.date: 08/09/2017
ms.topic: article
keywords: Windows 10, UWP, Uri, recorte, esboço
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: d9469dd6efd3598ab7abd9791a976385f4dfce49
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684659"
---
# <a name="launch-screen-snipping"></a>Iniciar a captura de tela

Os esquemas **MS-screenclip:** e **MS-screensketch:** URI permitem que você inicie capturas de tela de recorte ou edição.

## <a name="open-a-new-snip-from-your-app"></a>Abrir um novo recorte do seu aplicativo

O **MS-screenclip:** URI permite que seu aplicativo seja aberto automaticamente e inicie um novo recorte. O recorte resultante é copiado para a área de transferência do usuário, mas não é automaticamente passado de volta para o aplicativo de abertura.

**MS-screenclip:** usa os seguintes parâmetros:

| Parâmetro | Digite | Necessária | Descrição |
| --- | --- | --- | --- |
| source | sequência | não | Uma cadeia de caracteres de forma livre para indicar a origem que iniciou o URI. |
| delayInSeconds | int | não | Um valor inteiro, de 1 a 30. Especifica o atraso, em segundos totais, entre a chamada de URI e quando o recorte começa. |
| callbackformat | sequência | não | Este parâmetro não está disponível. |

## <a name="launching-the-snip--sketch-app"></a>Iniciando o aplicativo recorte & esboço

O URI **MS-screensketch:** permite que você inicie programaticamente o aplicativo recorte & esboço e abra uma imagem específica nesse aplicativo para anotação.

**MS-screensketch:** usa os seguintes parâmetros:

| Parâmetro | Digite | Necessária | Descrição |
| --- | --- | --- | --- |
| sharedAccessToken | sequência | não | Um token que identifica o arquivo a ser aberto no aplicativo recorte & esboço. Recuperado de [SharedStorageAccessManager. AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile). Se esse parâmetro for omitido, o aplicativo será iniciado sem a abertura de um arquivo. |
| secondarySharedAccessToken | sequência | não | Uma cadeia de caracteres que identifica um arquivo JSON com metadados sobre o recorte. Os metadados podem incluir um campo **clipPoints** com uma matriz de coordenadas x, y e/ou um [UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity). |
| source | sequência | não | Uma cadeia de caracteres de forma livre para indicar a origem que iniciou o URI. |
| isTemporary | bool | não | Se definido como true, o esboço da tela tentará excluir o arquivo depois de abri-lo. |

O exemplo a seguir chama o método [LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) para enviar uma imagem para o recorte & esboço do aplicativo do usuário.

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```

O exemplo a seguir ilustra o que um arquivo especificado pelo parâmetro **secondarySharedAccessToken** de **MS-screensketch** pode conter:

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
  "userActivity": "{\"$schema\":\"http://activity.windows.com/user-activity.json\",\"UserActivity\":\"type\",\"1.0\":\"version\",\"cross-platform-identifiers\":[{\"platform\":\"windows_universal\",\"application\":\"Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge\"},{\"platform\":\"host\",\"application\":\"edge.activity.windows.com\"}],\"activationUrl\":\"microsoft-edge:https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"contentUrl\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"visualElements\":{\"attribution\":{\"iconUrl\":\"https://www.microsoft.com/favicon.ico?v2\",\"alternateText\":\"microsoft.com\"},\"description\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"backgroundColor\":\"#FF0078D7\",\"displayText\":\"Use snipping tool to capture screenshots - Windows Help\",\"content\":{\"$schema\":\"http://adaptivecards.io/schemas/adaptive-card.json\",\"type\":\"AdaptiveCard\",\"version\":\"1.0\",\"body\":[{\"type\":\"Container\",\"items\":[{\"type\":\"TextBlock\",\"text\":\"Use snipping tool to capture screenshots - Windows Help\",\"weight\":\"bolder\",\"size\":\"large\",\"wrap\":true,\"maxLines\":3},{\"type\":\"TextBlock\",\"text\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"size\":\"normal\",\"wrap\":true,\"maxLines\":3}]}]}},\"isRoamable\":true,\"appActivityId\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\"}"
}

```
