---
ms.assetid: 3A404CC0-A997-45C8-B2E8-44745539759D
title: Permissões de acesso a arquivo
description: Os aplicativos podem acessar certos locais do sistema de arquivos por padrão. Os aplicativos também podem acessar outros locais por meio do seletor de arquivos ou da declaração de funcionalidades.
---
# Permissões de acesso a arquivo

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Os aplicativos podem acessar certos locais do sistema de arquivos por padrão. Os aplicativos também podem acessar outros locais por meio do seletor de arquivos ou da declaração de funcionalidades.

## Os locais que todos os aplicativos podem acessar

Ao criar um novo aplicativo, por padrão, você pode acessar os seguintes locais do sistema de arquivos:

-   **Diretório de instalação do aplicativo** A pasta onde o aplicativo está instalado no sistema do usuário.

    Há duas maneiras principais de acessar arquivos e pastas no diretório de instalação de seu aplicativo:

    1.  Você pode recuperar um [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) que represente o diretório de instalação do aplicativo, assim:
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        Windows.Storage.StorageFolder installedLocation = Windows.ApplicationModel.Package.Current.InstalledLocation;
        ```
        ```javascript
        var installDirectory = Windows.ApplicationModel.Package.current.installedLocation;
        ```

       Você pode acessar arquivos e pastas no diretório usando métodos [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230). No exemplo, esse **StorageFolder** é armazenado na variável `installDirectory`. Você pode saber mais sobre como trabalhar com o pacote do aplicativo e o diretório de instalação baixando o [Amostra de informações de pacote do aplicativo](http://go.microsoft.com/fwlink/p/?linkid=231526) para Windows 8.1 e reutilizando o código-fonte no aplicativo do Windows 10.

    2.  Você pode recuperar um arquivo diretamente do diretório de instalação do aplicativo usando um URI de aplicativo, assim:
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;            
        StorageFile file = await StorageFile.GetFileFromApplicationUriAsync("ms-appx:///file.txt");
        ```
        ```javascript
        Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appx:///file.txt").done(
            function(file) {
                // Process file
            }
        );
        ```
        
        Quando o [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) é concluído, ele retorna um [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que representa o arquivo *file.txt* no diretório de instalação do aplicativo (`file` no exemplo).

        O prefixo "ms-appx:///" no URI refere-se ao diretório de instalação do aplicativo. Você pode aprender mais sobre como usar os URIs em [Como usar URIs para conteúdo de referência](https://msdn.microsoft.com/library/windows/apps/hh781215).

    Além disso, diferentemente de outros locais, você também pode acessar os arquivos no diretório de instalação de seu aplicativo usando [Win32 e COM para aplicativos UWP (Plataforma Universal do Windows)](https://msdn.microsoft.com/library/windows/apps/br205757) e algumas [funções da Biblioteca Padrão do C/C++ do Microsoft Visual Studio](http://msdn.microsoft.com/library/hh875057.aspx).

    O diretório de instalação do aplicativo é um local somente leitura. Você não pode obter acesso ao diretório de instalação através do seletor de arquivos.

-   **Localizações de dados do aplicativo.** As pastas em que seu aplicativo pode armazenar dados. Essas pastas (local, móvel e temporária) são criadas quando o aplicativo é instalado.

    Há duas maneiras principais de acessar arquivos e pastas nos locais de dados de seu aplicativo:

    1.  Use as propriedades [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587) para recuperar uma pasta de dados do aplicativo.

        Por exemplo, você pode usar o [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587).[**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) para recuperar um [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) que represente a pasta local do seu aplicativo assim:
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFolder localFolder = ApplicationData.Current.LocalFolder;
        ```
        ```javascript
        var localFolder = Windows.Storage.ApplicationData.current.localFolder;
        ```
 
        Se você quiser acessar o roaming do seu aplicativo ou uma pasta temporária, use a propriedade [**RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623) ou [**TemporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629).

        Depois que você recuperar um [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) que represente um local de dados do aplicativo, você pode acessar arquivos e pastas nesse local usando os métodos **StorageFolder**. No exemplo, esses objetos **StorageFolder** são armazenados na variável `localFolder`. Você pode saber mais sobre como usar locais de dados de aplicativo em [Gerenciando dados de aplicativo](https://msdn.microsoft.com/library/windows/apps/hh465109) e baixando a [Amostra de dados de aplicativo](http://go.microsoft.com/fwlink/p/?linkid=231478) para Windows 8.1 e reutilizando seu código-fonte em seu aplicativo do Windows 10.

    2.  Por exemplo, você pode recuperar um arquivo diretamente da pasta local do seu aplicativo usando um URI de aplicativo, assim:
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFile file = await StorageFile.GetFileFromApplicationUriAsync("ms-appdata:///local/file.txt");
        ```
        ```javascript
        Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appdata:///local/file.txt").done(
            function(file) {
                // Process file
            }
        );
        ```

        Quando o [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) é concluído, ele retorna um [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que representa o arquivo *file.txt* na pasta local do aplicativo (`file` no exemplo).

        O prefixo "ms-appdata:///local/" no URI refere-se à pasta local do aplicativo. Para acessar os arquivos nas pastas roaming ou temporárias do aplicativo, use "ms-appdata:///roaming/" ou "ms-appdata:///temporary/" em vez disso. Você pode saber mais sobre como usar URIs do aplicativo em [Como carregar recursos de arquivos](https://msdn.microsoft.com/library/windows/apps/hh781229).

    Além disso, diferentemente de outros locais, você também pode acessar os arquivos nos locais de dados de seu aplicativo usando [Win32 e COM para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/br205757) e algumas funções da Biblioteca Padrão C/C++ do Visual Studio.

    Você não pode acessar as pastas locais, roaming ou temporária com o seletor de arquivos.

-   **Dispositivos removíveis.** Além disso, o aplicativo pode acessar alguns dos arquivos em dispositivos conectados por padrão. Esta é uma opção se o seu aplicativo usa a [extensão de Dispositivo AutoPlay](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh464906.aspx#autoplay) para ser iniciada automaticamente quando os usuários conectam um dispositivo, como uma câmera ou pen drive USB, ao seu sistema. Os arquivos que seu aplicativo podem acessar são limitados a determinados tipos de arquivos que são especificados através de declarações Associação de Tipo de Arquivos no manifesto do aplicativo.

    É claro que você também pode obter acesso a arquivos e pastas em um dispositivo removível chamando o seletor de arquivos (usando o [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) e o [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)) e permitindo que o usuário escolha os arquivos e pastas para seu aplicativo acessar. Saiba como usar o seletor de arquivos em [Abrir arquivos e pastas com um seletor](quickstart-using-file-and-folder-pickers.md).

    **Observação**  Para obter mais informações sobre como acessar um cartão SD em aplicativos móveis, consulte [Acessar o cartão SD](access-the-sd-card.md).

     

## Locais que os aplicativos da Windows Store podem acessar

-   **Pasta Downloads do usuário.** A pasta em que os arquivos baixados são salvos por padrão.

    Por padrão, o aplicativo só pode acessar arquivos e pastas na pasta de Downloads do usuário que seu aplicativo criou. No entanto, você pode ter acesso a arquivos e pastas na pasta Downloads do usuário chamando um seletor de arquivos ([**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) ou [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)) de modo que os usuários possam navegar e escolher arquivos ou pastas para seu aplicativo acessar.

    -   Você pode criar um arquivo na pasta Downloads do usuário, assim:
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFile newFile = await DownloadsFolder.CreateFileAsync("file.txt");
        ```
        ```javascript
        Windows.Storage.DownloadsFolder.createFileAsync("file.txt").done(
            function(newFile) {
                // Process file
            }
        );
        ```
 
        [
							O **DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh996761) é sobrecarregado de modo que você possa especificar o que o sistema deve fazer se já houver um arquivo existente na pasta Downloads com o mesmo nome. Quando esses métodos são concluídos, eles retornam um [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que representa o arquivo que foi criado. Esse arquivo é chamado `newFile` no exemplo.

    -   Você pode criar uma subpasta na pasta Downloads do usuário, assim:
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFolder newFolder = await DownloadsFolder.CreateFolderAsync("New Folder");
        ```
        ```javascript
        Windows.Storage.DownloadsFolder.createFolderAsync("New Folder").done(
            function(newFolder) {
                // Process folder
            }
        );
        ```
 
        [
							O **DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFolderAsync**](https://msdn.microsoft.com/library/windows/apps/hh996763) é sobrecarregado de modo que você possa especificar o que o sistema deve fazer se já houver uma subpasta existente na pasta Downloads com o mesmo nome. Quando esses métodos são concluídos, eles retornam um [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) que representa a subpasta que foi criada. Esse arquivo é chamado `newFolder` no exemplo.

    Se você criar um arquivo ou uma pasta na pasta Downloads, recomendamos adicionar esse item ao [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) do seu aplicativo, para que ele possa acessar prontamente esse item no futuro.

## Acessando locais adicionais

Além dos locais padrão, o aplicativo pode acessar arquivos e pastas adicionais declarando as funcionalidades no manifesto do aplicativo (veja [Declarações de funcionalidade do aplicativo](https://msdn.microsoft.com/library/windows/apps/mt270968)), ou chamando um seletor de arquivos para permitir que o usuário escolha os arquivos e as pastas para o aplicativo acessar (veja [Abrir arquivos e pastas com um seletor](quickstart-using-file-and-folder-pickers.md)).

A tabela a seguir lista locais adicionais que você pode acessar declarando os recursos e usando a API [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346) associada:

| Localização | Funcionalidade | API Windows.Storage |
|----------|------------|---------------------|
| Documentos | DocumentsLibrary <br><br>Observação: você deve adicionar Associações de tipo de arquivo ao manifesto do aplicativo que declarem tipos específicos de arquivos que seu aplicativo pode acessar neste local. <br><br>Use esse recurso se o seu aplicativo:<br>- Possibilitar acesso offline entre plataformas ao conteúdo específico do OneDrive usando URLs válidas do OneDrive ou IDs de Recursos corretas<br>- Salvar arquivos abertos no OneDrive do usuário automaticamente enquanto offline | [KnownFolders.DocumentsLibrary](https://msdn.microsoft.com/library/windows/apps/br227152) |
| Música     | MusicLibrary <br>Consulte também [Arquivos e pastas nas bibliotecas Música, Fotos e Vídeos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.MusicLibrary](https://msdn.microsoft.com/library/windows/apps/br227155) |    
| Imagens  | PicturesLibrary<br> Consulte também [Arquivos e pastas nas bibliotecas Música, Fotos e Vídeos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.PicturesLibrary](https://msdn.microsoft.com/library/windows/apps/br227156) |  
| Vídeos    | VideosLibrary<br>Consulte também [Arquivos e pastas nas bibliotecas Música, Fotos e Vídeos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.VideosLibrary](https://msdn.microsoft.com/library/windows/apps/br227159) |   
| Dispositivos removíveis  | RemovableDevices <br><br>Observação  Você deve adicionar Associações de tipo de arquivo ao manifesto do aplicativo que declarem tipos específicos de arquivos que seu aplicativo pode acessar neste local. <br><br>Consulte também [Acessar o cartão SD](access-the-sd-card.md). | [KnownFolders.RemovableDevices](https://msdn.microsoft.com/library/windows/apps/br227158) |  
| Bibliotecas de grupo doméstico  | Pelo menos um dos seguintes recursos é necessário. <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.HomeGroup](https://msdn.microsoft.com/library/windows/apps/br227153) |      
| Dispositivos do servidor de mídia (DLNA) | Pelo menos um dos seguintes recursos é necessário. <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.MediaServerDevices](https://msdn.microsoft.com/library/windows/apps/br227154) | 
| Pastas UNC (Convenção de Nomenclatura Universal) | Uma combinação dos seguintes recursos é necessária. <br><br>O recurso de redes de trabalho e domésticas: <br>- PrivateNetworkClientServer <br><br>E pelo menos um recurso de internet e de redes públicas: <br>- InternetClient <br>- InternetClientServer <br><br>E, se aplicável, o recurso de credenciais de domínio:<br>- EnterpriseAuthentication <br><br>Observação: você deve adicionar Associações de tipo de arquivo ao manifesto do aplicativo que declarem tipos específicos de arquivos que seu aplicativo pode acessar neste local. | Recupere uma pasta usando: <br>[StorageFolder.GetFolderFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227278) <br><br>Recupere um arquivo usando: <br>[StorageFile.GetFileFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227206) |



<!--HONumber=Mar16_HO1-->


