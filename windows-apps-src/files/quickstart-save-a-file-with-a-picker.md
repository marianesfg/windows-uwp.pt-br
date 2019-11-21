---
ms.assetid: 8BDDE64A-77D2-4F9D-A1A0-E4C634BCD890
title: Salvar um arquivo com um seletor
description: Use o FileSavePicker para permitir que os usuários especifiquem o nome e o local em que desejam que o aplicativo salve um arquivo.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2293907b3088890ac01d9037609054961aa95992
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259574"
---
# <a name="save-a-file-with-a-picker"></a>Salvar um arquivo com um seletor

**APIs importantes**

-   [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker)
-   [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)

Use o [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) para permitir que os usuários especifiquem o nome e o local onde desejam que o aplicativo salve um aplicativo.

> [!NOTE]
> Para obter um exemplo completo, veja o [Exemplo de seletor de arquivos](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FilePicker).

 

## <a name="prerequisites"></a>Pré-requisitos


-   **Entender a programação assíncrona para aplicativos UWP (Plataforma Universal do Windows)**

    Você pode aprender a escrever aplicativos assíncronos em C# ou Visual Basic, consulte [Chamar APIs assíncronas em C# ou Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Para saber como escrever aplicativos assíncronos em C++, consulte [Programação assíncrona em C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

-   **Permissões de acesso ao local**

    Consulte [Permissões de acesso a arquivo](file-access-permissions.md).

## <a name="filesavepicker-step-by-step"></a>FileSavePicker: passo a passo

Use um [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) para que os usuários possam especificar o nome, o tipo e o local de salvamento de um arquivo. Crie, personalize e mostre um objeto do seletor de arquivos e, em seguida, salve os dados pelo objeto [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) retornado que representa o arquivo selecionado.

1.  **Criar e personalizar o FileSavePicker**

    ```cs
    var savePicker = new Windows.Storage.Pickers.FileSavePicker();
    savePicker.SuggestedStartLocation =
        Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
    // Dropdown of file types the user can save the file as
    savePicker.FileTypeChoices.Add("Plain Text", new List<string>() { ".txt" });
    // Default file name if the user does not type one in or select a file to replace
    savePicker.SuggestedFileName = "New Document";
    ```

Defina propriedades no objeto do seletor de arquivos que forem relevantes para seus usuários e seu aplicativo. Este exemplo define três propriedades: [**SuggestedStartLocation**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.suggestedstartlocation), [**FileTypeChoices**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.filetypechoices) e [**SuggestedFileName**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.suggestedfilename).
     
- Como nosso usuário está salvando um documento ou arquivo de texto, a amostra define [**SuggestedStartLocation**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.suggestedstartlocation) como a pasta local do aplicativo usando [**LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder). Configure [**SuggestedStartLocation**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.suggestedstartlocation) para um local apropriado para o tipo de arquivo que será salvo, como Música, Imagens, Vídeos ou Documentos. A partir do local inicial, o usuário pode navegar para outros locais.

- Como desejamos que nosso aplicativo abra o arquivo depois dele ser salvo, usamos [**FileTypeChoices**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.filetypechoices) para especificar os tipos de arquivos que a amostra suporta (documentos do Microsoft Word e arquivos de texto). Certifique-se de que todos os tipos de arquivos sejam suportados por seu aplicativo. Os usuários poderão salvar seus arquivos como qualquer tipo de arquivo que você especificar. Eles também podem alterar o tipo de arquivo selecionando outro tipo de arquivo que você especificou. A primeira opção de tipo de arquivo na lista será selecionada por padrão: para controlar isso, configure a propriedade [**DefaultFileExtension**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.defaultfileextension).

    > [!NOTE]
    > O seletor de arquivos também usa o tipo de arquivo atualmente selecionado para filtrar quais arquivos são exibidos, de forma que somente os tipos de arquivo que correspondam aos tipos de arquivo selecionados sejam exibidos para o usuário.

- Para poupar o usuário da digitação, o exemplo define um [**SuggestedFileName**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.suggestedfilename). Torne seu nome de arquivo sugerido relevante para o arquivo que está sendo salvo. Por exemplo, como no Word, você pode sugerir o nome de arquivo existente se houver um, ou a primeira linha de um documento se o usuário estiver salvando um arquivo que ainda não possui nome.

> [!NOTE]
>Os objetos  [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) exibem o seletor de arquivos usando o modo de exibição [**PickerViewMode.List**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.PickerViewMode).

2.  **Mostrar o FileSavePicker e salvar no arquivo selecionado**

    Exiba o seletor de arquivos chamando [**PickSaveFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.picksavefileasync). Depois que o usuário especifica o nome, o tipo de arquivo e o local e confirma o arquivo a ser salvo, **PickSaveFileAsync** retorna um objeto [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) que representa o arquivo salvo. Você pode capturar e processar esse arquivo agora que tem acesso de leitura e gravação a ele.

    ```cs
    Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();
        if (file != null)
        {
            // Prevent updates to the remote version of the file until
            // we finish making changes and call CompleteUpdatesAsync.
            Windows.Storage.CachedFileManager.DeferUpdates(file);
            // write to file
            await Windows.Storage.FileIO.WriteTextAsync(file, file.Name);
            // Let Windows know that we're finished changing the file so
            // the other app can update the remote version of the file.
            // Completing updates may require Windows to ask for user input.
            Windows.Storage.Provider.FileUpdateStatus status =
                await Windows.Storage.CachedFileManager.CompleteUpdatesAsync(file);
            if (status == Windows.Storage.Provider.FileUpdateStatus.Complete)
            {
                this.textBlock.Text = "File " + file.Name + " was saved.";
            }
            else
            {
                this.textBlock.Text = "File " + file.Name + " couldn't be saved.";
            }
        }
        else
        {
            this.textBlock.Text = "Operation cancelled.";
        }
    ```

O exemplo verifica se o arquivo é válido e grava seu próprio nome de arquivo nele. Consulte também [Criando, escrevendo e lendo um arquivo](quickstart-reading-and-writing-files.md).

> [!TIP]
> Sempre verifique o arquivo salvo para ter certeza de sua validade antes de executar qualquer outro processamento. Depois disso, você poderá salvar o conteúdo no arquivo, conforme apropriado ao seu aplicativo, e fornecer o comportamento adequado se o arquivo selecionado não for válido.
