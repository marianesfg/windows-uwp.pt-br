---
author: laurenhughes
ms.assetid: 12ECEA89-59D2-4BCE-B24C-5A4DD525E0C7
title: Acessando o conteúdo do Grupo Doméstico
description: Acesse o conteúdo armazenado na pasta Grupo Doméstico do usuário, incluindo imagens, músicas e vídeos.
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 398c44db6a391008605ed6fa4dad877bcead035d
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6027200"
---
# <a name="accessing-homegroup-content"></a>Acessando o conteúdo do Grupo Doméstico



**APIs importantes**

-   [**Classe Windows.Storage.KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151)

Acesse o conteúdo armazenado na pasta Grupo Doméstico do usuário, incluindo imagens, músicas e vídeos.

## <a name="prerequisites"></a>Pré-requisitos

-   **Entender a programação assíncrona para aplicativos da Plataforma Universal do Windows (UWP)**

    Você pode aprender a escrever aplicativos assíncronos em C# ou Visual Basic, consulte [Chamar APIs assíncronas em C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Para saber como escrever aplicativos assíncronos em C++, consulte [Programação assíncrona em C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Declarações de recursos do aplicativo**

    Para acessar o conteúdo do Grupo Doméstico, a máquina do usuário deverá ter um Grupo Doméstico configurado e seu aplicativo deve ter pelo menos um dos seguintes recursos: **picturesLibrary**, **musicLibrary** ou **videosLibrary**. Quando seu aplicativo acessar a pasta Grupo Doméstico, ele verá somente as bibliotecas correspondentes aos recursos declarados em seu manifesto do aplicativo. Para saber mais, consulte [Permissões de acesso a arquivo](file-access-permissions.md).

    > [!NOTE]
    >  O conteúdo na biblioteca Documentos de um Grupo Doméstico não fica visível para seu aplicativo independentemente dos recursos declarados em seu manifesto do aplicativo e independente das configurações de compartilhamento do usuário.     

-   **Noções básicas para usar seletores de arquivos**

    Normalmente, você usa o seletor de arquivos para acessar arquivos e pastas no Grupo Doméstico. Para saber como usar o seletor de arquivos, consulte [Abrir arquivos e pastas com um seletor](quickstart-using-file-and-folder-pickers.md).

-   **Noções básicas sobre consultas de arquivo e pasta**

    Você pode usar consultas para enumerar arquivos e pastas no Grupo Doméstico. Para saber mais sobre as consultas de arquivo e pasta, consulte [Enumerando e consultando arquivos e pastas](quickstart-listing-files-and-folders.md).

## <a name="open-the-file-picker-at-the-homegroup"></a>Abrir o seletor de arquivos no Grupo Doméstico

Siga essas etapas para abrir uma instancia do seletor de arquivos que permite que o usuário selecione arquivos e pastas a partir do Grupo Doméstico:

1.  **Criar e personalizar o seletor de arquivos**

    Use o [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) para criar o seletor de arquivos e defina o [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207854) do seletor como [**PickerLocationId.HomeGroup**](https://msdn.microsoft.com/library/windows/apps/br207890). Ou defina outras propriedades que sejam relevantes para seus usuários e seu aplicativo. Para obter diretrizes para ajudá-lo a decidir como personalizar o seletor de arquivos, consulte [Diretrizes e lista de verificação para os seletores de arquivos](https://msdn.microsoft.com/library/windows/apps/hh465182)

    Este exemplo cria um seletor de arquivos que abre no Grupo Doméstico, inclui os arquivos de qualquer tipo e exibe os arquivos como imagens em miniatura:
    ```cs
    Windows.Storage.Pickers.FileOpenPicker picker = new Windows.Storage.Pickers.FileOpenPicker();
    picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
    picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.HomeGroup;
    picker.FileTypeFilter.Clear();
    picker.FileTypeFilter.Add("*");
    ```

2.  **Mostre o seletor de arquivos e processe o arquivo selecionado.**

    Depois de criar e personalizar um seletor de arquivos, permita que o usuário selecione um arquivo chamando o [**FileOpenPicker.PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275)ou vários arquivos chamando o [**FileOpenPicker.PickMultipleFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br207851).

    Este exemplo exibe o seletor de arquivos para permitir que o usuário selecione um arquivo:
    ```cs
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();

    if (file != null)
    {
        // Do something with the file.
    }
    else
    {
        // No file returned. Handle the error.
    }   
    ```

## <a name="search-the-homegroup-for-files"></a>Pesquisar o Grupo Doméstico para arquivos

Esta seção mostra como encontrar itens do Grupo Doméstico correspondentes a um termo da consulta fornecido pelo usuário.

1.  **Obter o termo da consulta do usuário.**

    Aqui, obtemos um termo da consulta que o usuário inseriu em um controle do [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) chamado `searchQueryTextBox`:
    ```cs
    string queryTerm = this.searchQueryTextBox.Text;    
    ```

2.  **Defina as opções de consulta e o filtro de pesquisa.**

    As opções de consulta determinam como os resultados da pesquisa são classificados, enquanto o filtro de pesquisa determina quais itens são incluídos nos resultados da pesquisa.

    Este exemplo define as opções de consulta que classificam os resultados da pesquisa por importância e depois a data é modificada. O filtro de pesquisa é o termo da consulta que o usuário inseriu na etapa anterior:
    ```cs
    Windows.Storage.Search.QueryOptions queryOptions =
            new Windows.Storage.Search.QueryOptions
                (Windows.Storage.Search.CommonFileQuery.OrderBySearchRank, null);
    queryOptions.UserSearchFilter = queryTerm.Text;
    Windows.Storage.Search.StorageFileQueryResult queryResults =
            Windows.Storage.KnownFolders.HomeGroup.CreateFileQueryWithOptions(queryOptions);    
    ```

3.  **Execute a consulta e processe os resultados.**

    O exemplo a seguir executa a consulta da pesquisa no Grupo Doméstico e salva os nomes de quaisquer arquivos correspondentes como uma lista de cadeia de caracteres.
    ```cs
    System.Collections.Generic.IReadOnlyList<Windows.Storage.StorageFile> files =
        await queryResults.GetFilesAsync();

    if (files.Count > 0)
    {
        outputString += (files.Count == 1) ? "One file found\n" : files.Count.ToString() + " files found\n";
        foreach (Windows.Storage.StorageFile file in files)
        {
            outputString += file.Name + "\n";
        }
    }    
    ```


## <a name="search-the-homegroup-for-a-particular-users-shared-files"></a>Pesquise o Grupo Doméstico para obter arquivos compartilhados de um usuário específico

Esta seção mostra a você como encontrar arquivos do Grupo Doméstico compartilhados por um usuário específico.

1.  **Obtenha uma coleção de usuários do Grupo Doméstico.**

    Cada uma das pastas de primeiro nível no Grupo Doméstico representa um usuário do Grupo Doméstico individual. Assim, para obter a coleção de usuários do Grupo Doméstico, chame [**GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br227279) para recuperar as pastas do Grupo Doméstico de nível superior.
    ```cs
    System.Collections.Generic.IReadOnlyList<Windows.Storage.StorageFolder> hgFolders =
        await Windows.Storage.KnownFolders.HomeGroup.GetFoldersAsync();    
    ```

2.  **Localize a pasta do usuário de destino e depois crie uma consulta de arquivo de escopo para essa pasta do usuário.**

    O exemplo a seguir interage por meio das pastas recuperadas para localizar a pasta do usuário de destino. Em seguida, ele define as opções de consulta para localizar todos os arquivos na pasta, classificados primeiro por relevância e depois pela data de modificação. O exemplo cria uma cadeia de caracteres que reporta o número de arquivos encontrados, juntamente com os nomes dos arquivos.
    ```cs
    bool userFound = false;
    foreach (Windows.Storage.StorageFolder folder in hgFolders)
    {
        if (folder.DisplayName == targetUserName)
        {
            // Found the target user's folder, now find all files in the folder.
            userFound = true;
            Windows.Storage.Search.QueryOptions queryOptions =
                new Windows.Storage.Search.QueryOptions
                    (Windows.Storage.Search.CommonFileQuery.OrderBySearchRank, null);
            queryOptions.UserSearchFilter = "*";
            Windows.Storage.Search.StorageFileQueryResult queryResults =
                folder.CreateFileQueryWithOptions(queryOptions);
            System.Collections.Generic.IReadOnlyList<Windows.Storage.StorageFile> files =
                await queryResults.GetFilesAsync();

            if (files.Count > 0)
            {
                string outputString = "Searched for files belonging to " + targetUserName + "'\n";
                outputString += (files.Count == 1) ? "One file found\n" : files.Count.ToString() + " files found\n";
                foreach (Windows.Storage.StorageFile file in files)
                {
                    outputString += file.Name + "\n";
                }
            }
        }
    }    
    ```

## <a name="stream-video-from-the-homegroup"></a>Transmitir vídeo a partir do Grupo Doméstico

Siga essas etapas para transmitir o conteúdo do vídeo a partir do Grupo Doméstico:

1.  **Incluir um MediaElement em seu aplicativo.**

    Um [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) permite que você reproduza conteúdo de áudio e vídeo em seu aplicativo. Para saber mais sobre reprodução de áudio e vídeo, consulte [Criar controles personalizados de transporte](https://msdn.microsoft.com/library/windows/apps/mt187271) e [Áudio, vídeo e câmera](https://msdn.microsoft.com/library/windows/apps/mt203788).
    ```HTML
    <Grid x:Name="Output" HorizontalAlignment="Left" VerticalAlignment="Top" Grid.Row="1">
        <MediaElement x:Name="VideoBox" HorizontalAlignment="Left" VerticalAlignment="Top" Margin="0" Width="400" Height="300"/>
    </Grid>    
    ```

2.  **Abra um seletor de arquivos no Grupo Doméstico e aplique um filtro que inclua os arquivos de vídeo nos formatos compatíveis com seu aplicativo.**

    Este exemplo inclui arquivos .mp4 e .wmv no seletor de arquivos aberto.
    ```cs
    Windows.Storage.Pickers.FileOpenPicker picker = new Windows.Storage.Pickers.FileOpenPicker();
    picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
    picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.HomeGroup;
    picker.FileTypeFilter.Clear();
    picker.FileTypeFilter.Add(".mp4");
    picker.FileTypeFilter.Add(".wmv");
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();   
    ```

3.  **Abra a seleção de arquivos do usuário para acesso de leitura e defina o fluxo do arquivo como a origem para o** [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)e, em seguida, executar o arquivo.
    ```cs
    if (file != null)
    {
        var stream = await file.OpenAsync(Windows.Storage.FileAccessMode.Read);
        VideoBox.SetSource(stream, file.ContentType);
        VideoBox.Stop();
        VideoBox.Play();
    }
    else
    {
        // No file selected. Handle the error here.
    }    
    ```
