---
author: TylerMSFT
title: Esquema ms-tonepicker
description: Este tópico descreve o esquema de URI ms-tonepicker e como usá-lo para exibir um seletor de tom para selecionar um tom, salvar um tom e obter o nome amigável para um tom.
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 0c17e4fb-7241-4da9-b457-d6d3a7aefccb
ms.localizationpriority: medium
ms.openlocfilehash: 61967f0aa81c49cb4e81b11bedac84c318be1840
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6970442"
---
# <a name="choose-and-save-tones-using-the-ms-tonepicker-uri-scheme"></a>Escolher e salvar tons usando o esquema de URI ms-tonepicker

Este tópico descreve como usar o esquema de URI **ms-tonepicker:**. Esse esquema de URI pode ser usado para:
- Determinar se o seletor de tom está disponível no dispositivo.
- Exibir o seletor de tom para listar toques, sons do sistema, tons de texto e sons de alarme disponíveis; e obter um token de tom que representa o som que o usuário selecionou.
- Exibir o protetor de tom, que usa um token de arquivo de som como entrada e salva-o no dispositivo. Os tons salvos estão disponíveis por meio do seletor de tom. Os usuários também podem dar um nome amigável ao tom.
- Converta um token de tom em seu nome amigável.

## <a name="ms-tonepicker-uri-scheme-reference"></a>Referência de esquema de URI ms-tonepicker:

Esse esquema de URI não passa argumentos via cadeia de caracteres de esquema URI, mas passa argumentos por meio de um [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx). Todas as cadeias de caracteres diferenciam maiúsculas de minúsculas.

As seções a seguir indicam quais argumentos devem ser transmitidos para realizar a tarefa especificada.

## <a name="task-determine-if-the-tone-picker-is-available-on-the-device"></a>Tarefa: determinar se o seletor de tom está disponível no dispositivo
```cs
var status = await Launcher.QueryUriSupportAsync(new Uri("ms-tonepicker:"),     
                                     LaunchQuerySupportType.UriForResults,
                                     "Microsoft.Tonepicker_8wekyb3d8bbwe");

if (status != LaunchQuerySupportStatus.Available)
{
    // the tone picker is not available
}
```

## <a name="task-display-the-tone-picker"></a>Tarefa: exibir o seletor de tom

Os argumentos que você pode passar para exibir o seletor de tom são os seguintes:

| Parâmetro | Tipo | Obrigatório | Valores possíveis | Descrição |
|-----------|------|----------|-------|-------------|
| Ação | string | sim | "PickRingtone" | Abre o seletor de tom. |
| CurrentToneFilePath | string | não | Um token de tom existente. | O tom a ser exibido como o tom atual no seletor de tom. Se esse valor não for definido, o tom primeiro na lista será selecionado por padrão.<br>Não, estritamente falando, isso é um caminho de arquivo. Você pode obter um valor adequado para `CurrenttoneFilePath` do valor `ToneToken` retornado do seletor de tom.  |
| TypeFilter | string | não | "Toques", "Notificações", "Alarmes", "None" | Seleciona quais tons serão adicionados ao seletor. Se nenhum filtro for especificado, todos os tons serão exibidos. |

Valores que são retornados em [LaunchUriResults.Result](https://msdn.microsoft.com/library/windows/apps/windows.system.launchuriresult.result.aspx):

| Valores de retorno | Tipo | Valores possíveis | Descrição |
|--------------|------|-------|-------------|
| Resultado | Int32 | 0 = êxito. <br>1 = cancelado. <br>7 = parâmetros inválidos. <br>8 = nenhum tom corresponde aos critérios de filtro. <br>255 = a ação especificada não foi implementada. | O resultado da operação de seletor. |
| ToneToken | string | Token do tom selecionado. <br>A cadeia de caracteres ficará vazia se o usuário selecionar **padrão** no seletor. | Esse token pode ser usado em uma carga de notificação do sistema, ou pode ser atribuído como toque ou tom do texto de um contato. O parâmetro será retornado no ValueSet somente se **Resultado** for 0. |
| DisplayName | string | Nome amigável do tom especificado. | Uma cadeia de caracteres que pode ser exibida ao usuário para representar o tom selecionado. O parâmetro será retornado no ValueSet somente se **Resultado** for 0. |


**Exemplo: abrir o seletor de tom para que o usuário possa selecionar um tom**

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

## <a name="task-display-the-tone-saver"></a>Tarefa: exibir o protetor de tom

Os argumentos que você pode passar para exibir o protetor de tom são os seguintes:

| Parâmetro | Tipo | Obrigatório | Valores possíveis | Descrição |
|-----------|------|----------|-------|-------------|
| Ação | string | sim | "SaveRingtone" | Abre o seletor para salvar um toque. |
| ToneFileSharingToken | string | sim | Token de compartilhamento de arquivo [SharedStorageAccessManager](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.aspx) para o arquivo de toque a ser salvo. | Salva um arquivo de som específico como um toque. Os tipos de conteúdo com suporte para o arquivo são áudio mpeg e áudio x-ms-wma. |
| DisplayName | string | não | Nome amigável do tom especificado. | Define o nome de exibição a ser usado ao salvar o toque especificado. |

Valores que são retornados em [LaunchUriResults.Result](https://msdn.microsoft.com/library/windows/apps/windows.system.launchuriresult.result.aspx):

| Valores de retorno | Tipo | Valores possíveis | Descrição |
|--------------|------|-------|-------------|
| Resultado | Int32 | 0 = êxito.<br>1 = cancelado pelo usuário.<br>2 = arquivo inválido.<br>3 = tipo de conteúdo de arquivo inválido.<br>4 = o arquivo excede o tamanho máximo de toque (1 MB no Windows 10).<br>5 = o arquivo excede o limite de duração de 40 segundos.<br>6 = o arquivo está protegido pelo gerenciamento de direitos digitais.<br>7 = parâmetros inválidos. | O resultado da operação de seletor. |

**Exemplo: salvar um arquivo de música local como um toque**

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

## <a name="task-convert-a-tone-token-to-its-friendly-name"></a>Tarefa: converter um token de tom em seu nome amigável

Os argumentos que você pode passar para obter o nome amigável de um tom são os seguintes:

| Parâmetro | Tipo | Obrigatório | Valores possíveis | Descrição |
|-----------|------|----------|-------|-------------|
| Ação | string | sim | "GetToneName" | Indica que você deseja obter o nome amigável de um tom. |
| ToneToken | string | sim | O token de tom | O token de tom de onde obter um nome de exibição. |

Valores que são retornados em [LaunchUriResults.Result](https://msdn.microsoft.com/library/windows/apps/windows.system.launchuriresult.result.aspx):

| Valor de retorno | Tipo | Valores possíveis | Descrição |
|--------------|------|-------|-------------|
| Resultado | Int32 | 0 = operação de seletor bem-sucedida.<br>7 = parâmetro incorreto (por exemplo, nenhum ToneToken fornecido).<br>9 = erro ao ler o nome do token especificado.<br>10 = não é possível encontrar o token de tom especificado. | O resultado da operação de seletor.
| DisplayName | string | Nome amigável do tom. | Retorna o nome de exibição do tom selecionado. Esse parâmetro será retornado no ValueSet somente se **Resultado** for 0. |

**Exemplo: recuperar um token de tom de Contact.RingToneToken e exibir o nome amigável no cartão de visita.**

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
