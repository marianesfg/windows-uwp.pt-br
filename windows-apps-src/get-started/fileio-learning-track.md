---
author: TylerMSFT
title: Trabalhar com arquivos
description: Saiba como trabalhar com arquivos na Plataforma Universal do Windows.
ms.author: twhitney
ms.date: 05/01/2018
ms.topic: article
keywords: introdução, uwp, windows 10, acompanhamento de aprendizado, arquivos, io de arquivo, ler arquivo, gravar arquivo, criar arquivo, escrever texto, ler texto
ms.localizationpriority: medium
ms.openlocfilehash: 02ac4d3f157343182097e67dc8825dde940302c3
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6666292"
---
# <a name="work-with-files"></a>Trabalhar com arquivos

Este tópico abrange o que você precisa saber para começar a leitura e gravação de arquivos na Plataforma Universal do Windows (UWP). As principais APIs e os tipos são introduzidos, e os links são fornecidos para ajudar você a saber mais.

Isso não é um tutorial. Se você quiser um tutorial, veja [Criar, gravar e ler um arquivo](https://docs.microsoft.com/windows/uwp/files/quickstart-reading-and-writing-files) que, além de demonstrar como criar, ler e gravar um arquivo, mostra como usar buffers e fluxos. Você também pode estar interessado o [Exemplo de acesso ao arquivo](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess), que mostra como criar, ler, gravar, copiar e excluir um arquivo, bem como recuperar propriedades de arquivo e lembre-se de um arquivo ou pasta para que seu aplicativo pode acessá-lo facilmente novamente.

Vamos examinar o código para gravar e ler o texto de um arquivo, e como acessar as pastas locais, em roaming e temporárias do aplicativo.

## <a name="what-do-you-need-to-know"></a>O que você precisa saber

Veja os principais tipos que você precisa conhecer para ler ou gravar texto de/para um arquivo:

- [Windows.Storage.StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) representa um arquivo. Essa classe tem propriedades que fornecem informações sobre o arquivo e métodos para criar, abrir, copiar, excluir e renomear arquivos.
Você pode estar acostumado a lidar com caminhos de cadeia de caracteres. Há algumas APIs da UWP que usam uma cadeia de caracteres, mas com mais frequência, você usará um **StorageFile** para representar um arquivo, pois alguns trabalhados na UWP podem não ter um caminho ou podem ter um caminho complicado. Use [StorageFile.GetFileFromPathAsync()](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefrompathasync) a fim de converter um caminho de cadeia de caracteres para um **StorageFile**. 

- A classe [FileIO](https://docs.microsoft.com/uwp/api/windows.storage.fileio) fornece uma maneira fácil de ler e gravar texto. Essa classe também pode fazer a leitura/gravação de uma matriz de bytes ou do conteúdo de um buffer. Essa classe é muito parecida com a classe [PathIO](https://docs.microsoft.com/uwp/api/windows.storage.pathio). A principal diferença é que em vez de usar um caminho de cadeia de caracteres, como **PathIO**, ele usa um **StorageFile**.
- [Windows.Storage.StorageFolder](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder) representa uma pasta (diretório). Essa classe tem métodos para criar arquivos, consultar o conteúdo de uma pasta, criar, renomear e excluir pastas, além de propriedades que fornecem informações sobre uma pasta. 

As maneiras comuns de obter um **StorageFolder** incluem:

- [Windows.Storage.Pickers.FolderPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.folderpicker) que permite ao usuário navegar até a pasta que desejam usar.
- [Windows.Storage.ApplicationData.Current](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current) que fornece o **StorageFolder** específico para uma das pastas locais do aplicativo, como a pasta local, em roaming e temporária.
- [Windows.Storage.KnownFolders](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders) que fornece a **StorageFolder** para bibliotecas conhecidas, como as bibliotecas de Música ou Imagens.

## <a name="write-text-to-a-file"></a>Gravar texto em um arquivo

 Para essa introdução, o foco será em um cenário simples: a leitura e gravação de texto. Vamos começar analisando alguns códigos que usam a classe **FileIO** para gravar texto em um arquivo.

```csharp
Windows.Storage.StorageFolder storageFolder = Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile file = await storageFolder.CreateFileAsync("test.txt",
        Windows.Storage.CreationCollisionOption.OpenIfExists);

await Windows.Storage.FileIO.WriteTextAsync(file, "Example of writing a string\r\n");

// Append a list of strings, one per line, to the file
var listOfStrings = new List<string> { "line1", "line2", "line3" };
await Windows.Storage.FileIO.AppendLinesAsync(file, listOfStrings); // each entry in the list is written to the file on its own line.
```

Primeiro, identifique onde o arquivo deve estar localizado. `Windows.Storage.ApplicationData.Current.LocalFolder` fornece acesso à pasta de dados local, que é criada para o aplicativo durante a instalação. Consulte [Acessar o sistema de arquivos](#access-the-file-system) para obter detalhes sobre as pastas que o aplicativo pode acessar.

Em seguida, usamos **StorageFolder** para criar o arquivo (ou abri-lo se já existir).

A classe **FileIO** fornece uma maneira conveniente de gravar texto no arquivo. `FileIO.WriteTextAsync()` substitui o conteúdo inteiro do arquivo pelo texto fornecido. `FileIO.AppendLinesAsync()` acrescenta uma coleção de cadeias de caracteres ao arquivo, escrevendo uma cadeia de caracteres por linha.

## <a name="read-text-from-a-file"></a>Ler texto de um arquivo

Assim como acontece na gravação, a leitura começa ao especificar onde o arquivo está localizado. Vamos usar o mesmo local como no exemplo acima. Em seguida, vamos usar a classe **FileIO** para ler o conteúdo.

```csharp
Windows.Storage.StorageFolder storageFolder = Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile file = await storageFolder.GetFileAsync("test.txt");

string text = await Windows.Storage.FileIO.ReadTextAsync(file);
```

Você também pode ler cada linha do arquivo em cadeias de caracteres individuais em uma coleção com `IList<string> contents = await Windows.Storage.FileIO.ReadLinesAsync(sampleFile);`

## <a name="access-the-file-system"></a>Acessar o sistema de arquivos

Na plataforma UWP, o acesso à pasta é restrito para garantir a integridade e a privacidade dos dados do usuário. 

### <a name="app-folders"></a>Pastas de aplicativo

Quando um aplicativo UWP é instalado, várias pastas são criadas em c:\users\<nome do usuário>\AppData\Local\Packages\<identificador do pacote de aplicativo>\ para armazenar os arquivos locais, em roaming e temporário do aplicativo, entre outras coisas. O aplicativo não precisa declarar as funcionalidades para acessar essas pastas, que não podem ser acessadas por outros aplicativos. Essas pastas também são removidas quando o aplicativo é desinstalado.

Veja algumas das pastas de aplicativo que você normalmente usará:

- **LocalState**: para dados locais do dispositivo atual. Quando é feito o backup do dispositivo, os dados nesse diretório são salvos em uma imagem de backup no OneDrive. Se o usuário redefinir ou substituir o dispositivo, os dados serão restaurados. Acesse essa pasta com `Windows.Storage.ApplicationData.Current.LocalFolder.`Salve os dados locais que você não deseja que estejam no backup do OneDrive em **LocalCacheFolder**, que podem ser acessados com `Windows.Storage.ApplicationData.Current.LocalCacheFolder`.

- **RoamingState**: para dados que devem ser replicados em todos os dispositivos nos quais o aplicativo está instalado. O Windows limita a quantidade de dados em roaming, portanto, salve somente as configurações do usuário e pequenos arquivos aqui. Acesse a pasta roaming com `Windows.Storage.ApplicationData.Current.RoamingFolder`.

- **TempState**: para dados que podem ser excluídos quando o aplicativo não estiver em execução. Acesse essa pasta com `Windows.Storage.ApplicationData.Current.TemporaryFolder`.

### <a name="access-the-rest-of-the-file-system"></a>Acessar o resto sistema de arquivos

Um aplicativo UWP deve declarar sua intenção para acessar uma biblioteca de usuário específica, adicionando a funcionalidade correspondente ao seu manifesto. Em seguida, quando o aplicativo estiver instalado, o usuário é solicitado a confirmar a autorização para acessar a biblioteca especificada. Caso contrário, o aplicativo não é instalado. Há recursos para acessar bibliotecas de música, fotos e vídeos. Consulte [Declaração de funcionalidade do aplicativo](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) para ver uma lista completa. Para obter um **StorageFolder** dessas bibliotecas, use a classe [Windows.Storage.KnownFolders](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders).

#### <a name="documents-library"></a>Biblioteca de documentos

Embora não exista uma funcionalidade para acessar a biblioteca de documentos do usuário, essa funcionalidade é restrita, ou seja, um aplicativo que a declarar será rejeitado pela Microsoft Store, exceto quando você segue um processo para obter uma aprovação especial. Ela não serve para uso geral. Em vez disso, use os seletores de arquivo ou pasta (veja [Abrir arquivos e pastas com um seletor](https://docs.microsoft.com/windows/uwp/files/quickstart-using-file-and-folder-pickers) e [Salvar um arquivo com um seletor](https://docs.microsoft.com/windows/uwp/files/quickstart-save-a-file-with-a-picker)) que permitem ao usuário navegar até a pasta ou o arquivo. Quando o usuário navega até um arquivo ou pasta, ele permite implicitamente que o aplicativo o acesse e o sistema permite o acesso.

#### <a name="general-access"></a>Acesso geral

Como alternativa, o aplicativo pode declarar a funcionalidade restrita [broadFileSystem](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) em seu manifesto, que também requer aprovação da Microsoft Store. Em seguida, o aplicativo pode acessar qualquer arquivo ao qual o usuário tenha acesso sem a intervenção de um seletor de arquivo ou pasta.

Para obter uma lista completa dos locais que os aplicativos podem acessar, consulte [Permissões de acesso ao arquivo](https://docs.microsoft.com/windows/uwp/files/file-access-permissions).

## <a name="useful-apis-and-docs"></a>APIs e documentos úteis

Veja um resumo rápido de APIs e outras documentações úteis para ajudar você a começar a trabalhar com arquivos e pastas.

### <a name="useful-apis"></a>APIs úteis

| API | Descrição |
|------|---------------|
|  [Windows.Storage.StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) | Fornece informações sobre o arquivo e métodos para criar, abrir, copiar, excluir e renomear arquivos. |
| [Windows.Storage.StorageFolder](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder) | Fornece informações sobre a pasta, métodos para criar arquivos e métodos para criar, renomear, e excluir pastas. |
| [FileIO](https://docs.microsoft.com/uwp/api/windows.storage.fileio) |  Fornece uma maneira fácil de ler e gravar texto. Essa classe também pode fazer a leitura/gravação de uma matriz de bytes ou do conteúdo de um buffer. |
| [PathIO](https://docs.microsoft.com/uwp/api/windows.storage.pathio) | Fornece uma maneira fácil de fazer a leitura/gravação de/para um arquivo considerando um caminho de cadeia de caracteres dele. Essa classe também pode fazer a leitura/gravação de uma matriz de bytes ou do conteúdo de um buffer. |
| [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader) & [DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) |  Ler e gravar buffers, bytes, inteiros, GUIDs, TimeSpans e muito mais de/para um fluxo. |
| [Windows.Storage.ApplicationData.Current](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current) | Fornece acesso às pastas criadas para o aplicativo, como a pasta local, de roaming e arquivos temporários. |
| [Windows.Storage.Pickers.FolderPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.folderpicker) |  Permite que o usuário escolha uma pasta e retorna um **StorageFolder** para ele. Isso é como você pode obter acesso aos locais que o aplicativo não pode acessar por padrão. |
| [Windows.Storage.Pickers.FileOpenPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker) | Permite que o usuário escolha um arquivo para abrir e retorna um **StorageFile** para ele. Isso é como você pode obter acesso a um arquivo que o aplicativo não pode acessar por padrão. |
| [Windows.Storage.Pickers.FileSavePicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker) | Permite que o usuário escolha o nome, a extensão e o local de armazenamento para um arquivo. Retorna um **StorageFile**. Isso é como você pode salvar um arquivo em um local que o aplicativo não pode acessar por padrão. |
|  [Namespace Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/windows.storage.streams) | Abrange a leitura e gravação de fluxos. Analise especificamente as classes [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader) e [DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) que fazem a leitura e gravação de buffers, bytes, inteiros, GUIDs, TimeSpans e muito mais. |

### <a name="useful-docs"></a>Documentos úteis

| Tópico | Descrição |
|-------|----------------|
| [Namespace do Windows.Storage](https://docs.microsoft.com/uwp/api/windows.storage) | Documentos de referência do API. |
| [Arquivos, pastas e bibliotecas](https://docs.microsoft.com/windows/uwp/files/) | Documentos conceituais. |
| [Criar, gravar e ler um arquivo](https://docs.microsoft.com/windows/uwp/files/quickstart-reading-and-writing-files) | Capas de criação, leitura e gravação de texto, dados binários e fluxos. |
| [Introdução ao armazenamento de dados de aplicativo localmente](https://blogs.windows.com/buildingapps/2016/05/10/getting-started-storing-app-data-locally/#pCbJKGjcShh5DTV5.97) | Além de cobrir as práticas recomendadas para salvar os dados locais, abrange a finalidade da pasta LocalSettings e LocalCache. |
| [Introdução aos Dados de aplicativo em roaming](https://blogs.windows.com/buildingapps/2016/05/03/getting-started-with-roaming-app-data/#RgjgLt5OkU9DbVV8.97) | Uma série de duas partes sobre como usar dados de aplicativo em roaming. |
| [Diretrizes de dados de aplicativo em roaming](http://msdn.microsoft.com/library/windows/apps/hh465094) | Siga essas diretrizes de dados em roaming ao projetar seu aplicativo. |
| [Armazenar e recuperar configurações e outros dados de aplicativo](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data) | Fornece uma visão geral de várias fontes de dados de aplicativo, como as pastas locais, em roaming e temporárias. Consulte a seção [Dados em roaming](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data#roaming-data) para ver diretrizes e mais informações sobre a gravação de dados em roaming entre os dispositivos. |
| [Permissões de acesso a arquivo](https://docs.microsoft.com/windows/uwp/files/file-access-permissions) | Informações sobre quais locais do sistema de arquivo que o aplicativo pode acessar. |
| [Abrir arquivos e pastas com um seletor](https://docs.microsoft.com/windows/uwp/files/quickstart-using-file-and-folder-pickers) | Mostra como acessar arquivos e pastas permitindo que o usuário decida por uma interface do usuário do seletor. |
| [Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/windows.storage.streams) | Tipos usados para ler e gravar fluxos. |
| [Arquivos e pastas nas bibliotecas de Música, Imagens e Vídeos](https://docs.microsoft.com/windows/uwp/files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries) | Aborda como remover pastas de bibliotecas, obter a lista de pastas em uma biblioteca e descobrir fotos, músicas e vídeos armazenados. |

## <a name="useful-code-samples"></a>Exemplos de código úteis

| Exemplo de código | Descrição |
|-----------------|---------------|
| [Amostra de dados de aplicativos](https://code.msdn.microsoft.com/windowsapps/ApplicationData-sample-fb043eb2) | Mostra como armazenar e recuperar dados específicos de cada usuário usando as APIs de dados de aplicativos. |
| [Exemplo de acesso a arquivos](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) | Mostra como criar, ler, gravar, copiar e excluir um arquivo. |
| [Exemplo do seletor de arquivos](http://code.msdn.microsoft.com/windowsapps/File-picker-sample-9f294cba) | Mostra como acessar arquivos e pastas, permitindo que o usuário as escolha com a interface do usuário e como salvar um arquivo para que o usuário possa especificar o nome, o tipo e o local de um arquivo para salvar. |
| [Amostra de JSON](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Json) | Mostra como codificar e decodificar objetos JavaScript Object Notation (JSON), matrizes, cadeias de caracteres, números e boolianos usando o [namespace Windows.Data.Json](https://docs.microsoft.com/uwp/api/Windows.Data.Json). |
| [Exemplos de código adicionais](https://developer.microsoft.com//windows/samples) | Escolha **Arquivos, pastas e bibliotecas** na lista suspensa categoria. |