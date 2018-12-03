---
ms.assetid: 3A404CC0-A997-45C8-B2E8-44745539759D
title: Permissões de acesso a arquivo
description: Os aplicativos podem acessar certos locais do sistema de arquivos por padrão. Os aplicativos também podem acessar outros locais por meio do seletor de arquivos ou da declaração de funcionalidades.
ms.date: 06/28/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d960235e73ea9172fb966f227af9440923f3553e
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8461643"
---
# <a name="file-access-permissions"></a>Permissões de acesso a arquivo

Os aplicativos Universais do Windows (apps) podem acessar determinados locais do sistema de arquivos por padrão. Os aplicativos também podem acessar outros locais por meio do seletor de arquivos ou da declaração de funcionalidades.

## <a name="the-locations-that-all-apps-can-access"></a>Os locais que todos os aplicativos podem acessar

Ao criar um novo aplicativo, por padrão, você pode acessar os seguintes locais do sistema de arquivos:

### <a name="application-install-directory"></a>Diretório de instalação de aplicativo
A pasta onde seu aplicativo está instalado no sistema do usuário.

Há duas maneiras principais de acessar arquivos e pastas em seu aplicativo diretório de instalação:

1. Você pode recuperar um [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) que represente o diretório de instalação do aplicativo, assim:

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

Você pode acessar arquivos e pastas no diretório usando métodos [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230). No exemplo, esse **StorageFolder** é armazenado na variável `installDirectory`. Você pode saber mais sobre como trabalhar com o pacote do app e o diretório de instalação em [Exemplo de informações de pacote do app](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Package) no GitHub.

2. Você pode recuperar um arquivo diretamente do diretório de instalação do aplicativo usando um URI de aplicativo, assim:

```cs
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

Quando o [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) é concluído, ele retorna um [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que representa o arquivo `file.txt` no diretório de instalação do app (no exemplo, `file`).

O prefixo "ms-appx:///" no URI refere-se ao diretório de instalação do aplicativo. Você pode aprender mais sobre como usar os URIs em [Como usar URIs para conteúdo de referência](https://msdn.microsoft.com/library/windows/apps/hh781215).

Além disso, diferentemente de outros locais, você também pode acessar os arquivos no diretório de instalação de seu aplicativo usando [Win32 e COM para aplicativos UWP (Plataforma Universal do Windows)](https://msdn.microsoft.com/library/windows/apps/br205757) e algumas [funções da Biblioteca Padrão do C/C++ do Microsoft Visual Studio](http://msdn.microsoft.com/library/hh875057.aspx).

O diretório de instalação do aplicativo é um local somente leitura. Você não pode obter acesso ao diretório de instalação através do seletor de arquivos.

### <a name="application-data-locations"></a>Locais de dados de aplicativo
As pastas em que seu aplicativo pode armazenar dados. Essas pastas (local, móvel e temporária) são criadas quando o aplicativo é instalado.

Há duas maneiras principais de acessar arquivos e pastas de locais de dados do seu aplicativo:

1.  Use as propriedades [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587) para recuperar uma pasta de dados do aplicativo.

Por exemplo, você pode usar o [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587).[**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) para recuperar um [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) que represente a pasta local do seu aplicativo assim:

```cs
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

Se você quiser acessar o roaming do seu aplicativo ou uma pasta temporária, use a propriedade [**RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623) ou [**TemporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629).

Depois que você recuperar um [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) que represente um local de dados do aplicativo, você pode acessar arquivos e pastas nesse local usando os métodos **StorageFolder**. No exemplo, esses objetos **StorageFolder** são armazenados na variável `localFolder`. Você pode saber mais sobre como usar locais de dados de app nas diretrizes da página [Classe ApplicationData](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata) e ao baixar a [Exemplo de dados de app](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData) do GitHub.

2. Por exemplo, você pode recuperar um arquivo diretamente da pasta local do seu aplicativo usando um URI de aplicativo, assim:

```cs
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

Quando o [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) é concluído, ele retorna um [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que representa o arquivo `file.txt` na pasta local do app (no exemplo, `file`).

O prefixo "ms-appdata:///local/" no URI refere-se à pasta local do aplicativo. Para acessar os arquivos nas pastas roaming ou temporárias do aplicativo, use "ms-appdata:///roaming/" ou "ms-appdata:///temporary/" em vez disso. Você pode saber mais sobre como usar URIs do aplicativo em [Como carregar recursos de arquivos](https://msdn.microsoft.com/library/windows/apps/hh781229).

Além disso, diferentemente de outros locais, você também pode acessar os arquivos nos locais de dados de seu aplicativo usando [Win32 e COM para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/br205757) e algumas funções da Biblioteca Padrão C/C++ do Visual Studio.

Você não pode acessar as pastas locais, roaming ou temporárias o seletor de arquivos.

### <a name="removable-devices"></a>Dispositivos removíveis
Além disso, o aplicativo pode acessar alguns dos arquivos em dispositivos conectados por padrão. Esta é uma opção se o seu aplicativo usa a [extensão de Dispositivo AutoPlay](https://msdn.microsoft.com/library/windows/apps/xaml/hh464906.aspx#autoplay) para ser iniciada automaticamente quando os usuários conectam um dispositivo, como uma câmera ou pen drive USB, ao seu sistema. Os arquivos que seu aplicativo podem acessar são limitados a determinados tipos de arquivos que são especificados através de declarações Associação de Tipo de Arquivos no manifesto do aplicativo.

É claro que você também pode obter acesso a arquivos e pastas em um dispositivo removível chamando o seletor de arquivos (usando o [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) e o [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)) e permitindo que o usuário escolha os arquivos e pastas para seu aplicativo acessar. Saiba como usar o seletor de arquivos em [Abrir arquivos e pastas com um seletor](quickstart-using-file-and-folder-pickers.md).

> [!NOTE]
> Para saber mais sobre como acessar um cartão SD ou outros dispositivos removíveis, veja [Acessar o cartão SD](access-the-sd-card.md).

## <a name="locations-that-uwp-apps-can-access"></a>Locais que podem acessar aplicativos UWP
### <a name="users-downloads-folder"></a>Pasta Downloads do usuário

A pasta em que os arquivos baixados são salvos por padrão.

Por padrão, o aplicativo só pode acessar arquivos e pastas na pasta de Downloads do usuário que seu aplicativo criou. No entanto, você pode ter acesso a arquivos e pastas na pasta Downloads do usuário chamando um seletor de arquivos ([**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) ou [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)) de modo que os usuários possam navegar e escolher arquivos ou pastas para seu aplicativo acessar.

- Você pode criar um arquivo na pasta Downloads do usuário desta forma:

```cs
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

[**DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh996761) é sobrecarregado de modo que você possa especificar o que o sistema deve fazer se já houver um arquivo existente na pasta Downloads com o mesmo nome. Quando esses métodos são concluídos, eles retornam um [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que representa o arquivo que foi criado. Esse arquivo é chamado `newFile` no exemplo.

- Você pode criar uma subpasta na pasta Downloads do usuário desta forma:

```cs
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

[**DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFolderAsync**](https://msdn.microsoft.com/library/windows/apps/hh996763) sobrecarregado de modo que você possa especificar o que o sistema deve fazer se já houver uma subpasta existente na pasta Downloads com o mesmo nome. Quando esses métodos são concluídos, eles retornam um [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) que representa a subpasta que foi criada. Esse arquivo é chamado `newFolder` no exemplo.

Se você criar um arquivo ou uma pasta na pasta Downloads, recomendamos adicionar esse item ao [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) do seu aplicativo, para que ele possa acessar prontamente esse item no futuro.

## <a name="accessing-additional-locations"></a>Acessando locais adicionais

Além dos locais padrão, o aplicativo pode acessar arquivos e pastas adicionais declarando as funcionalidades no manifesto do aplicativo (veja [Declarações de funcionalidades do app](https://msdn.microsoft.com/library/windows/apps/mt270968)), ou chamando um seletor de arquivos para permitir que o usuário escolha os arquivos e as pastas para o aplicativo acessar (veja [Abrir arquivos e pastas com um seletor](quickstart-using-file-and-folder-pickers.md)).

Os aplicativos que declaram a extensão [AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) têm permissões de sistema de arquivos do diretório de onde são iniciados na janela do console e para baixo.

A tabela a seguir lista locais adicionais que você pode acessar declarando os recursos e usando a API [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346) associada:

| Localização | Funcionalidade | API Windows.Storage |
|----------|------------|---------------------|
| Todos os arquivos aos quais o usuário tem acesso. Por exemplo: documentos, imagens, fotos, downloads, área de trabalho, OneDrive etc. | broadFileSystemAccess<br><br>Esta é uma funcionalidade restrita. No primeiro uso, o sistema solicitará que o usuário permita o acesso. O acesso é configurável em Configurações > Privacidade > Sistema de arquivos. Se você enviar para a Store um app que declare essa funcionalidade, precisará fornecer descrições adicionais do motivo pelo qual seu app precisa dessa funcionalidade, e como ele pretende usá-la.<br>Esse recurso funciona para as APIs no namespace [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346). | n/d |
| Documentos | DocumentsLibrary <br><br>Observação: você deve adicionar Associações de tipo de arquivo ao manifesto do aplicativo que declarem tipos específicos de arquivos que seu aplicativo pode acessar neste local. <br><br>Use esse recurso se o seu aplicativo:<br>- Possibilitar acesso offline entre plataformas ao conteúdo específico do OneDrive usando URLs válidas do OneDrive ou IDs de Recursos corretas<br>-Salvar abre arquivos no OneDrive do usuário automaticamente enquanto offline | [KnownFolders.DocumentsLibrary](https://msdn.microsoft.com/library/windows/apps/br227152) |
| Música     | MusicLibrary <br>Consulte também [Arquivos e pastas nas bibliotecas Música, Fotos e Vídeos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.MusicLibrary](https://msdn.microsoft.com/library/windows/apps/br227155) |    
| Imagens  | PicturesLibrary<br> Consulte também [Arquivos e pastas nas bibliotecas Música, Fotos e Vídeos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.PicturesLibrary](https://msdn.microsoft.com/library/windows/apps/br227156) |  
| Vídeos    | VideosLibrary<br>Consulte também [Arquivos e pastas nas bibliotecas Música, Fotos e Vídeos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.VideosLibrary](https://msdn.microsoft.com/library/windows/apps/br227159) |   
| Dispositivos removíveis  | RemovableDevices <br><br>Observação  Você deve adicionar Associações de tipo de arquivo ao manifesto do aplicativo que declarem tipos específicos de arquivos que seu aplicativo pode acessar neste local. <br><br>Consulte também [Acessar o cartão SD](access-the-sd-card.md). | [KnownFolders.RemovableDevices](https://msdn.microsoft.com/library/windows/apps/br227158) |  
| Bibliotecas de grupo doméstico  | Pelo menos um dos seguintes recursos é necessário. <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.HomeGroup](https://msdn.microsoft.com/library/windows/apps/br227153) |      
| Dispositivos do servidor de mídia (DLNA) | Pelo menos um dos seguintes recursos é necessário. <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.MediaServerDevices](https://msdn.microsoft.com/library/windows/apps/br227154) |
| Pastas UNC (Convenção de Nomenclatura Universal) | Uma combinação dos seguintes recursos é necessária. <br><br>O recurso de redes de trabalho e domésticas: <br>- PrivateNetworkClientServer <br><br>E pelo menos um recurso de internet e de redes públicas: <br>- InternetClient <br>- InternetClientServer <br><br>E, se aplicável, o recurso de credenciais de domínio:<br>- EnterpriseAuthentication <br><br>Observação: você deve adicionar Associações de tipo de arquivo ao manifesto do aplicativo que declarem tipos específicos de arquivos que seu aplicativo pode acessar neste local. | Recupere uma pasta usando: <br>[StorageFolder.GetFolderFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227278) <br><br>Recupere um arquivo usando: <br>[StorageFile.GetFileFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227206) |

**Exemplo**

Este exemplo adiciona a funcionalidade `broadFileSystemAccess` restrita. Além de especificar a funcionalidade, o namespace `rescap` deve ser adicionado, e também é adicionado a `IgnorableNamespaces`:

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
> Para obter uma lista completa de funcionalidades do app, veja [Declarações de funcionalidades do app](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).
