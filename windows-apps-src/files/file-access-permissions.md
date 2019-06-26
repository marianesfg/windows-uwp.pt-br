---
ms.assetid: 3A404CC0-A997-45C8-B2E8-44745539759D
title: Permissões de acesso a arquivo
description: Os aplicativos podem acessar certos locais do sistema de arquivos por padrão. Os aplicativos também podem acessar outros locais por meio do seletor de arquivos ou da declaração de funcionalidades.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- javascript
ms.openlocfilehash: 1473d93bc10f50bf361f92f753adb786e502fc3a
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66369431"
---
# <a name="file-access-permissions"></a>Permissões de acesso a arquivo

Os aplicativos UWP (Plataforma Universal do Windows) podem acessar determinados locais do sistema de arquivos por padrão. Os aplicativos também podem acessar outros locais por meio do seletor de arquivos ou da declaração de funcionalidades.

## <a name="the-locations-that-all-apps-can-access"></a>Os locais que todos os aplicativos podem acessar

Ao criar um novo aplicativo, por padrão, você pode acessar os seguintes locais do sistema de arquivos:

### <a name="application-install-directory"></a>Diretório de instalação do aplicativo
A pasta em que o aplicativo está instalado no sistema do usuário.

Há duas maneiras principais de acessar arquivos e pastas no diretório de instalação de seu aplicativo:

1. Você pode recuperar um [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) que represente o diretório de instalação do aplicativo, assim:

    ```csharp
    Windows.Storage.StorageFolder installedLocation = Windows.ApplicationModel.Package.Current.InstalledLocation;
    ```
    
    ```javascript
    var installDirectory = Windows.ApplicationModel.Package.current.installedLocation;
    ```
    
    ```cppwinrt
    #include <winrt/Windows.Storage.h>
    ...
    Windows::Storage::StorageFolder installedLocation{ Windows::ApplicationModel::Package::Current().InstalledLocation() };
    ```
    
    ```cpp
    Windows::Storage::StorageFolder^ installedLocation = Windows::ApplicationModel::Package::Current->InstalledLocation;
    ```

    Você pode acessar arquivos e pastas no diretório usando métodos [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder). No exemplo, esse **StorageFolder** é armazenado na variável `installDirectory`. Você pode saber mais sobre como trabalhar com o pacote do aplicativo e o diretório de instalação em [Exemplo de informações de pacote do aplicativo](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Package) no GitHub.

2. Você pode recuperar um arquivo diretamente do diretório de instalação do aplicativo usando um URI de aplicativo, assim:

    ```csharp
    using Windows.Storage;            
    StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///file.txt"));
    ```
    
    ```javascript
    Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appx:///file.txt").done(
        function(file) {
            // Process file
        }
    );
    ```
    
    ```cppwinrt
    Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
    {
        Windows::Storage::StorageFile file{
            co_await Windows::Storage::StorageFile::GetFileFromApplicationUriAsync(Windows::Foundation::Uri{L"ms-appx:///file.txt"})
        };
        // Process file
    }
    ```
    
    ```cpp
    auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appx:///file.txt")));
    getFileTask.then([](StorageFile^ file) 
    {
        // Process file
    });
    ```

    Quando o [**GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync) é concluído, ele retorna um [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) que representa o arquivo `file.txt` no diretório de instalação do aplicativo (no exemplo, `file`).
    
    O prefixo "ms-appx:///" no URI refere-se ao diretório de instalação do aplicativo. Você pode aprender mais sobre como usar os URIs em [Como usar URIs para conteúdo de referência](https://docs.microsoft.com/previous-versions/windows/apps/hh781215(v=win.10)).

Além disso, diferentemente de outros locais, você também pode acessar os arquivos no diretório de instalação de seu aplicativo usando [Win32 e COM para aplicativos UWP (Plataforma Universal do Windows)](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps) e algumas [funções da Biblioteca Padrão do C/C++ do Microsoft Visual Studio](https://docs.microsoft.com/cpp/cpp/c-cpp-language-and-standard-libraries).

O diretório de instalação do aplicativo é um local somente leitura. Você não pode obter acesso ao diretório de instalação através do seletor de arquivos.

### <a name="application-data-locations"></a>Localizações de dados de aplicativos
As pastas em que seu aplicativo pode armazenar dados. Essas pastas (local, móvel e temporária) são criadas quando o aplicativo é instalado.

Há duas maneiras principais de acessar arquivos e pastas nos locais de dados de seu aplicativo:

1.  Use as propriedades [**ApplicationData**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData) para recuperar uma pasta de dados do aplicativo.

    Por exemplo, você pode usar o [**ApplicationData**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData).[**LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder) para recuperar um [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) que represente a pasta local do seu aplicativo assim:
    
    ```csharp
    using Windows.Storage;
    StorageFolder localFolder = ApplicationData.Current.LocalFolder;
    ```
    
    ```javascript
    var localFolder = Windows.Storage.ApplicationData.current.localFolder;
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFolder storageFolder{
        Windows::Storage::ApplicationData::Current().LocalFolder()
    };
    ```
    
    ```cpp
    using namespace Windows::Storage;
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    ```
    
    Se você quiser acessar o roaming do seu aplicativo ou uma pasta temporária, use a propriedade [**RoamingFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder) ou [**TemporaryFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder).
    
    Depois que você recuperar um [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) que represente um local de dados do aplicativo, você pode acessar arquivos e pastas nesse local usando os métodos **StorageFolder**. No exemplo, esses objetos **StorageFolder** são armazenados na variável `localFolder`. Você pode saber mais sobre como usar locais de dados de aplicativos nas diretrizes da página [Classe ApplicationData](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata) e ao baixar o [Exemplo de dados de aplicativos](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData) do GitHub.

2. Você pode recuperar um arquivo diretamente da pasta local do seu aplicativo usando um URI de aplicativo, assim:
    
    ```csharp
    using Windows.Storage;
    StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appdata:///local/file.txt"));
    ```
    
    ```javascript
    Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appdata:///local/file.txt").done(
        function(file) {
            // Process file
        }
    );
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFile file{
        co_await Windows::Storage::StorageFile::GetFileFromApplicationUriAsync(Windows::Foundation::Uri{ L"ms-appdata:///local/file.txt" })
    };
    // Process file
    ```
    
    ```cpp
    using Windows::Storage;
    auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appdata:///local/file.txt")));
    getFileTask.then([](StorageFile^ file) 
    {
        // Process file
    });
    ```
    
    Quando o [**GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync) é concluído, ele retorna um [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) que representa o arquivo `file.txt` na pasta local do aplicativo(no exemplo, `file`).
    
    O prefixo "ms-appdata:///local/" no URI refere-se à pasta local do aplicativo. Para acessar os arquivos nas pastas roaming ou temporárias do aplicativo, use "ms-appdata:///roaming/" ou "ms-appdata:///temporary/" em vez disso. Você pode saber mais sobre como usar URIs do aplicativo em [Como carregar recursos de arquivos](https://docs.microsoft.com/previous-versions/windows/apps/hh781229(v=win.10)).

Além disso, diferentemente de outros locais, você também pode acessar os arquivos nos locais de dados de seu aplicativo usando [Win32 e COM para aplicativos UWP](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps) e algumas funções da Biblioteca Padrão C/C++ do Visual Studio.

Você não pode acessar as pastas locais, roaming ou temporária com o seletor de arquivos.

### <a name="removable-devices"></a>Dispositivos removíveis
Além disso, o aplicativo pode acessar alguns dos arquivos em dispositivos conectados por padrão. Esta é uma opção se o seu aplicativo usa a [extensão de Dispositivo AutoPlay](https://docs.microsoft.com/previous-versions/windows/apps/hh464906(v=win.10)) para ser iniciada automaticamente quando os usuários conectam um dispositivo, como uma câmera ou pen drive USB, ao seu sistema. Os arquivos que seu aplicativo podem acessar são limitados a determinados tipos de arquivos que são especificados através de declarações Associação de Tipo de Arquivos no manifesto do aplicativo.

É claro que você também pode obter acesso a arquivos e pastas em um dispositivo removível chamando o seletor de arquivos (usando o [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) e o [**FolderPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FolderPicker)) e permitindo que o usuário escolha os arquivos e pastas para seu aplicativo acessar. Saiba como usar o seletor de arquivos em [Abrir arquivos e pastas com um seletor](quickstart-using-file-and-folder-pickers.md).

> [!NOTE]
> Para saber mais sobre como acessar um cartão SD ou outros dispositivos removíveis, veja [Acessar o cartão SD](access-the-sd-card.md).

## <a name="locations-that-uwp-apps-can-access"></a>Locais que aplicativos UWP podem acessar
### <a name="users-downloads-folder"></a>Pasta Downloads do usuário

A pasta em que os arquivos baixados são salvos por padrão.

Por padrão, o aplicativo só pode acessar arquivos e pastas na pasta de Downloads do usuário que seu aplicativo criou. No entanto, você pode ter acesso a arquivos e pastas na pasta Downloads do usuário chamando um seletor de arquivos ([**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) ou [**FolderPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FolderPicker)) de modo que os usuários possam navegar e escolher arquivos ou pastas para seu aplicativo acessar.

- Você pode criar um arquivo na pasta Downloads do usuário desta forma:

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
    
    ```cppwinrt
    Windows::Storage::StorageFile newFile{
        co_await Windows::Storage::DownloadsFolder::CreateFileAsync(L"file.txt")
    };
    // Process file
    ```
    
    ```cpp
    using Windows::Storage;
    auto createFileTask = create_task(DownloadsFolder::CreateFileAsync(L"file.txt"));
    createFileTask.then([](StorageFile^ newFile)
    {
        // Process file
    });
    ```

    [**DownloadsFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.DownloadsFolder).[**CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.downloadsfolder.createfileasync) é sobrecarregado de modo que você possa especificar o que o sistema deverá fazer se já houver um arquivo existente na pasta Downloads com o mesmo nome. Quando esses métodos são concluídos, eles retornam um [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) que representa o arquivo que foi criado. Esse arquivo é chamado `newFile` no exemplo.

- Você pode criar uma subpasta na pasta Downloads do usuário desta forma:

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
    
    ```cppwinrt
    Windows::Storage::StorageFolder newFolder{
        co_await Windows::Storage::DownloadsFolder::CreateFolderAsync(L"New Folder")
    };
    // Process folder
    ```
    
    ```cpp
    using Windows::Storage;
    auto createFolderTask = create_task(DownloadsFolder::CreateFolderAsync(L"New Folder"));
    createFolderTask.then([](StorageFolder^ newFolder)
    {
        // Process folder
    });
    ```

    [**DownloadsFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.DownloadsFolder).[**CreateFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.downloadsfolder.createfolderasync) é sobrecarregado de modo que você possa especificar o que o sistema deverá fazer se já houver uma subpasta existente na pasta Downloads com o mesmo nome. Quando esses métodos são concluídos, eles retornam um [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) que representa a subpasta que foi criada. Esse arquivo é chamado `newFolder` no exemplo.

Se você criar um arquivo ou uma pasta na pasta Downloads, recomendamos adicionar esse item ao [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) do seu aplicativo, para que ele possa acessar prontamente esse item no futuro.

## <a name="accessing-additional-locations"></a>Acessando locais adicionais

Além dos locais padrão, o aplicativo pode acessar arquivos e pastas adicionais declarando as funcionalidades no manifesto do aplicativo (veja [Declarações de funcionalidades do app](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)), ou chamando um seletor de arquivos para permitir que o usuário escolha os arquivos e as pastas para o aplicativo acessar (veja [Abrir arquivos e pastas com um seletor](quickstart-using-file-and-folder-pickers.md)).

Os aplicativos que declaram a extensão [AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) têm permissões do sistema de arquivos do diretório do qual são iniciados na janela do console e nos subdiretórios.

A tabela a seguir lista locais adicionais que você pode acessar declarando os recursos e usando a API [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage) associada:

| Location | Capacidade | API Windows.Storage |
|----------|------------|---------------------|
| Todos os arquivos aos quais o usuário tem acesso. Por exemplo: documentos, imagens, fotos, downloads, área de trabalho, OneDrive etc. | broadFileSystemAccess<br><br>Esta é uma funcionalidade restrita. O acesso é configurável em **Configurações** > **Privacidade** > **Sistema de arquivos**. Como os usuários podem conceder ou negar a permissão a qualquer momento em **Configurações**, você deve garantir que seu aplicativo seja resiliente a essas alterações. Se você achar que seu aplicativo não tem acesso, você poderá optar por solicitar que o usuário altere a configuração fornecendo um link para o artigo [Acesso e privacidade do sistema de arquivos do Windows 10](https://privacy.microsoft.com/en-US/windows-10-file-system-access-and-privacy). Observe que o usuário deve fechar o aplicativo, mudar a configuração e reiniciar o aplicativo. Se a configuração for mudada enquanto o aplicativo estiver em execução, a plataforma suspenderá o aplicativo para que você possa salvar o estado e, em seguida, forçará o encerramento do aplicativo para aplicar a nova configuração. Na atualização de abril de 2018, o padrão para as permissões é Ativada. Na atualização de outubro de 2018, o padrão é Desativada.<br /><br />Se você enviar um aplicativo que declare essa funcionalidade para a Store, precisará fornecer descrições adicionais do motivo pelo qual seu aplicativo precisa dessa funcionalidade e como ele pretende usá-la.<br>Essa funcionalidade funciona para APIs no namespace [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage). Consulte a seção **Exemplo** no final deste artigo para obter um exemplo de como habilitar essa funcionalidade em seu aplicativo. | n/d |
| Documentos | DocumentsLibrary <br><br>Observação: você deve adicionar Associações de tipo de arquivo ao manifesto do aplicativo que declarem tipos específicos de arquivos que seu aplicativo pode acessar neste local. <br><br>Use esse recurso se o seu aplicativo:<br>- Possibilitar acesso offline entre plataformas ao conteúdo específico do OneDrive usando URLs válidas do OneDrive ou IDs de Recursos corretas<br>– Salvar arquivos abertos no OneDrive do usuário automaticamente enquanto offline | [KnownFolders.DocumentsLibrary](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.documentslibrary) |
| Música     | MusicLibrary <br>Consulte também [Arquivos e pastas nas bibliotecas Música, Fotos e Vídeos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.MusicLibrary](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.musiclibrary) |    
| Imagens  | PicturesLibrary<br> Consulte também [Arquivos e pastas nas bibliotecas Música, Fotos e Vídeos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.PicturesLibrary](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.pictureslibrary) |  
| Vídeos    | VideosLibrary<br>Consulte também [Arquivos e pastas nas bibliotecas Música, Fotos e Vídeos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.VideosLibrary](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.videoslibrary) |   
| Dispositivos removíveis  | RemovableDevices <br><br>Observação  Você deve adicionar Associações de tipo de arquivo ao manifesto do aplicativo que declarem tipos específicos de arquivos que seu aplicativo pode acessar neste local. <br><br>Consulte também [Acessar o cartão SD](access-the-sd-card.md). | [KnownFolders.RemovableDevices](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.removabledevices) |  
| Bibliotecas de grupo doméstico  | Pelo menos um dos seguintes recursos é necessário. <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.HomeGroup](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.homegroup) |      
| Dispositivos do servidor de mídia (DLNA) | Pelo menos um dos seguintes recursos é necessário. <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.MediaServerDevices](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.mediaserverdevices) |
| Pastas UNC (Convenção de Nomenclatura Universal) | Uma combinação dos seguintes recursos é necessária. <br><br>O recurso de redes de trabalho e domésticas: <br>- PrivateNetworkClientServer <br><br>E pelo menos um recurso de internet e de redes públicas: <br>- InternetClient <br>- InternetClientServer <br><br>E, se aplicável, o recurso de credenciais de domínio:<br>- EnterpriseAuthentication <br><br>Observação: você deve adicionar Associações de tipo de arquivo ao manifesto do aplicativo que declarem tipos específicos de arquivos que seu aplicativo pode acessar neste local. | Recupere uma pasta usando: <br>[StorageFolder.GetFolderFromPathAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfolderfrompathasync) <br><br>Recupere um arquivo usando: <br>[StorageFile.GetFileFromPathAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefrompathasync) |

**Exemplo**

Este exemplo adiciona a funcionalidade `broadFileSystemAccess` restrita. Além de especificar a funcionalidade, o namespace `rescap` deve ser adicionado e também é adicionado a `IgnorableNamespaces`:

```xaml
<Package
  ...
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp uap5 rescap">
...
<Capabilities>
    <rescap:Capability Name="broadFileSystemAccess" />
</Capabilities>
```

> [!NOTE]
> Para obter uma lista completa de funcionalidades do aplicativo, veja [Declarações de funcionalidades do aplicativo](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).
