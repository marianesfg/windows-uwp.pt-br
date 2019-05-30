---
title: Esquema ms-tonepicker
description: Este tópico descreve o esquema de URI ms-tonepicker e como usá-lo para exibir um seletor de tom para selecionar e salvar um tom e obter o nome amigável para ele.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 0c17e4fb-7241-4da9-b457-d6d3a7aefccb
ms.localizationpriority: medium
ms.openlocfilehash: 78ed118ba15f38f8914cf2046344d782cd0df71b
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370792"
---
# <a name="choose-and-save-tones-using-the-ms-tonepicker-uri-scheme"></a>Escolher e salvar tons usando o esquema de URI ms-tonepicker

Este tópico descreve como usar o **tonepicker ms:** Esquema de URI. Esse esquema de URI pode ser usado para:
- Determinar se o seletor de tom está disponível no dispositivo.
- Exibir o seletor de tom para listar toques, sons do sistema, tons de texto e sons de alarme disponíveis; e obter um token de tom que representa o som que o usuário selecionou.
- Exibir o protetor de tom, que usa um token de arquivo de som como entrada e salva-o no dispositivo. Os tons salvos estão disponíveis por meio do seletor de tom. Os usuários também podem dar um nome amigável ao tom.
- Converta um token de tom em seu nome amigável.

## <a name="ms-tonepicker-uri-scheme-reference"></a>ms-tonepicker: Referência de esquema URI

Esse esquema de URI não passa argumentos via cadeia de caracteres de esquema URI, mas passa argumentos por meio de um [ValueSet](https://docs.microsoft.com/uwp/api/windows.foundation.collections.valueset). Todas as cadeias de caracteres diferenciam maiúsculas de minúsculas.

As seções a seguir indicam quais argumentos devem ser transmitidos para realizar a tarefa especificada.

## <a name="task-determine-if-the-tone-picker-is-available-on-the-device"></a>Tarefa: Determinar se o seletor de tom está disponível no dispositivo
```cs
var status = await Launcher.QueryUriSupportAsync(new Uri("ms-tonepicker:"),     
                                     LaunchQuerySupportType.UriForResults,
                                     "Microsoft.Tonepicker_8wekyb3d8bbwe");

if (status != LaunchQuerySupportStatus.Available)
{
    // the tone picker is not available
}
```

## <a name="task-display-the-tone-picker"></a>Tarefa: Exibir o seletor de tom

Os argumentos que você pode passar para exibir o seletor de tom são os seguintes:

| Parâmetro | Tipo | Obrigatório | Valores possíveis | Descrição |
|-----------|------|----------|-------|-------------|
| Ação | cadeia de caracteres | sim | "PickRingtone" | Abre o seletor de tom. |
| CurrentToneFilePath | cadeia de caracteres | não | Um token de tom existente. | O tom a ser exibido como o tom atual no seletor de tom. Se esse valor não for definido, o tom primeiro na lista será selecionado por padrão.<br>Não, estritamente falando, isso é um caminho de arquivo. Você pode obter um valor adequado para `CurrenttoneFilePath` do valor `ToneToken` retornado do seletor de tom.  |
| TypeFilter | cadeia de caracteres | não | "Toques", "Notificações", "Alarmes", "None" | Seleciona quais tons serão adicionados ao seletor. Se nenhum filtro for especificado, todos os tons serão exibidos. |

Valores que são retornados em [LaunchUriResults.Result](https://docs.microsoft.com/uwp/api/windows.system.launchuriresult.result):

| Valores de retorno | Tipo | Valores possíveis | Descrição |
|--------------|------|-------|-------------|
| Resultado | Int32 | 0 = êxito. <br>1 = cancelado. <br>7 = parâmetros inválidos. <br>8 = nenhum tom corresponde aos critérios de filtro. <br>255 = a ação especificada não foi implementada. | O resultado da operação de seletor. |
| ToneToken | cadeia de caracteres | Token do tom selecionado. <br>A cadeia de caracteres ficará vazia se o usuário selecionar **padrão** no seletor. | Esse token pode ser usado em uma carga de notificação do sistema, ou pode ser atribuído como toque ou tom do texto de um contato. O parâmetro será retornado no ValueSet somente se **Resultado** for 0. |
| DisplayName | cadeia de caracteres | Nome amigável do tom especificado. | Uma cadeia de caracteres que pode ser exibida ao usuário para representar o tom selecionado. O parâmetro será retornado no ValueSet somente se **Resultado** for 0. |


**Exemplo: Abra o seletor de tom, para que o usuário pode selecionar um tom**

``` cs
LauncherOptions options = new LauncherOptions();
options.TargetApplicationPackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";

ValueSet inputData = new ValueSet() {
    { "Action", "PickRingtone" },
    { "TypeFilter", "Ringtones" } // Show only ringtones
};

LaunchUriResult result = await Launcher.LaunchUriForResultsAsync(new Uri("ms-tonepicker:"), options, inputData);

if (result.Status == LaunchUriStatus.Success)
{
     Int32 resultCode =  (Int32)result.Result["Result"];
     if (resultCode == 0)
     {
         string token = result.Result["ToneToken"] as string;
         string name = result.Result["DisplayName"] as string;
     }
     else
     {
           // handle failure
     }
}
```

## <a name="task-display-the-tone-saver"></a>Tarefa: Exibir a proteção de tom

Os argumentos que você pode passar para exibir o protetor de tom são os seguintes:

| Parâmetro | Tipo | Obrigatório | Valores possíveis | Descrição |
|-----------|------|----------|-------|-------------|
| Ação | cadeia de caracteres | sim | "SaveRingtone" | Abre o seletor para salvar um toque. |
| ToneFileSharingToken | cadeia de caracteres | sim | Token de compartilhamento de arquivo [SharedStorageAccessManager](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager) para o arquivo de toque a ser salvo. | Salva um arquivo de som específico como um toque. Os tipos de conteúdo com suporte para o arquivo são áudio mpeg e áudio x-ms-wma. |
| DisplayName | cadeia de caracteres | não | Nome amigável do tom especificado. | Define o nome de exibição a ser usado ao salvar o toque especificado. |

Valores que são retornados em [LaunchUriResults.Result](https://docs.microsoft.com/uwp/api/windows.system.launchuriresult.result):

| Valores de retorno | Tipo | Valores possíveis | Descrição |
|--------------|------|-------|-------------|
| Resultado | Int32 | 0 = êxito.<br>1 = cancelado pelo usuário.<br>2 = arquivo inválido.<br>3 = tipo de conteúdo de arquivo inválido.<br>4 = o arquivo excede o tamanho máximo de toque (1 MB no Windows 10).<br>5 = o arquivo excede o limite de duração de 40 segundos.<br>6 = o arquivo está protegido pelo gerenciamento de direitos digitais.<br>7 = parâmetros inválidos. | O resultado da operação de seletor. |

**Exemplo: Salvar um arquivo de música local como um toque**

``` cs
LauncherOptions options = new LauncherOptions();
options.TargetApplicationPackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";

ValueSet inputData = new ValueSet() {
    { "Action", "SaveRingtone" },
    { "ToneFileSharingToken", SharedStorageAccessManager.AddFile(myLocalFile) }
};

LaunchUriResult result = await Launcher.LaunchUriForResultsAsync(new Uri("ms-tonepicker:"), options, inputData);

if (result.Status == LaunchUriStatus.Success)
{
     Int32 resultCode = (Int32)result.Result["Result"];

     if (resultCode == 0)
     {
         // no issues
     }
     else
     {
          switch (resultCode)
          {
             case 2:
              // The specified file was invalid
              break;
              case 3:
              // The specified file's content type is invalid
              break;
              case 4:
              // The specified file was too big
              break;
              case 5:
              // The specified file was too long
              break;
              case 6:
              // The file was protected by DRM
              break;
              case 7:
              // The specified parameter was incorrect
              break;
          }
      }
 }
```

## <a name="task-convert-a-tone-token-to-its-friendly-name"></a>Tarefa: Converter um token de tom em seu nome amigável

Os argumentos que você pode passar para obter o nome amigável de um tom são os seguintes:

| Parâmetro | Tipo | Obrigatório | Valores possíveis | Descrição |
|-----------|------|----------|-------|-------------|
| Ação | cadeia de caracteres | sim | "GetToneName" | Indica que você deseja obter o nome amigável de um tom. |
| ToneToken | cadeia de caracteres | sim | O token de tom | O token de tom de onde obter um nome de exibição. |

Valores que são retornados em [LaunchUriResults.Result](https://docs.microsoft.com/uwp/api/windows.system.launchuriresult.result):

| Retornar valor | Tipo | Valores possíveis | Descrição |
|--------------|------|-------|-------------|
| Resultado | Int32 | 0 = operação de seletor bem-sucedida.<br>7 = parâmetro incorreto (por exemplo, nenhum ToneToken fornecido).<br>9 = erro ao ler o nome do token especificado.<br>10 = não é possível encontrar o token de tom especificado. | O resultado da operação de seletor.
| DisplayName | cadeia de caracteres | Nome amigável do tom. | Retorna o nome de exibição do tom selecionado. Esse parâmetro será retornado no ValueSet somente se **Resultado** for 0. |

**Exemplo: Recuperar um token de tom do Contact.RingToneToken e exibir seu nome amigável do cartão de visita.**

```cs
using (var connection = new AppServiceConnection())
{
    connection.AppServiceName = "ms-tonepicker-nameprovider";
    connection.PackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";
    AppServiceConnectionStatus connectionStatus = await connection.OpenAsync();
    if (connectionStatus == AppServiceConnectionStatus.Success)
    {
        var message = new ValueSet() {
            { "Action", "GetToneName" },
            { "ToneToken", token)
        };
        AppServiceResponse response = await connection.SendMessageAsync(message);
        if (response.Status == AppServiceResponseStatus.Success)
        {
            Int32 resultCode = (Int32)response.Message["Result"];
            if (resultCode == 0)
            {
                string name = response.Message["DisplayName"] as string;
            }
            else
            {
                // handle failure
            }
        }
        else
        {
            // handle failure
        }
    }
}
```
