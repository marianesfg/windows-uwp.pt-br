---
description: Por trás de sua interface do usuário estão as camadas de negócios e dados.
title: Portando camadas de dados e de negócios WPSL para UWP
ms.assetid: 27c66759-2b35-41f5-9f7a-ceb97f4a0e3f
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9548026f0cae4ac414da15ad4ad2aa86f6226cbc
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74734911"
---
#  <a name="porting-windowsphone-silverlight-business-and-data-layers-to-uwp"></a>Portando Windows Phone camadas de dados e de negócios do Silverlight para UWP


O tópico anterior foi [Portabilidade para E/S, dispositivo e modelo de aplicativo](wpsl-to-uwp-input-and-sensors.md).

Por trás de sua interface do usuário estão as camadas de negócios e dados. O código dessas camadas chama as APIs do sistema operacional e do .NET Framework (por exemplo, processamento em segundo plano, localização, a câmera, o sistema de arquivos, rede e outros acessos a dados). A maioria delas está [disponível para um aplicativo da Plataforma Universal do Windows (UWP)](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10)), portanto, espere poder fazer a portabilidade de grande parte desse código sem alteração.

## <a name="asynchronous-methods"></a>Métodos assíncronos

Uma das prioridades da Plataforma Universal do Windows (UWP) é habilitar você para criar aplicativos que sejam real e consistentemente responsivos. As animações são sempre suaves e as interações de toque, tais como movimento panorâmico e passagem do dedo, são instantâneas e sem latência, fazendo parecer que a interface do usuário está colada ao seu dedo. Para conseguir isso, qualquer API da UWP que não possa garantir a conclusão de um processo dentro de 50 ms se torna assíncrona e seu nome recebe o sufixo **Async**. Seu thread de interface do usuário retornará imediatamente após a chamada de um método **Async** e o trabalho será realizado em outro thread. Tornou-se muito fácil consumir um método **Async**, sintaticamente, usando o operador C# **await**, objetos de promessa JavaScript e continuações C++. (Para obter mais informações, consulte [Programação assíncrona](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps).)

## <a name="background-processing"></a>Processamento em segundo plano

Um aplicativo do Windows Phone Silverlight pode usar um objeto **ScheduledTaskAgent** gerenciado para executar uma tarefa enquanto o aplicativo não está em primeiro plano. Um aplicativo UWP usa a classe [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) para criar e registrar uma tarefa em segundo plano de maneira semelhante. Você define uma classe que implementa o trabalho de sua tarefa em segundo plano. O sistema executa sua tarefa em segundo plano periodicamente, chamando o método [**Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) da classe para executar o trabalho. Em um aplicativo UWP, lembre-se de definir a declaração **Tarefas em Segundo Plano** no manifesto do pacote do aplicativo. Para obter mais informações, consulte [Ofereça suporte ao seu aplicativo com tarefas em segundo plano](https://docs.microsoft.com/windows/uwp/launch-resume/support-your-app-with-background-tasks).

Para transferir arquivos de dados grandes em segundo plano, um aplicativo Windows Phone Silverlight usa a classe **BackgroundTransferService** . Um aplicativo UWP usa APIs no namespace [**Windows.Networking.BackgroundTransfer**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer) para fazer isso. Os recursos usam um padrão semelhante para iniciar transferências, mas a nova API melhorou as funcionalidades e o desempenho. Para obter mais informações, consulte [Transferindo dados em segundo plano](https://docs.microsoft.com/previous-versions/windows/apps/hh452975(v=win.10)).

Um aplicativo Windows Phone do Silverlight usa as classes gerenciadas no namespace **Microsoft. Phone. BackgroundAudio** para reproduzir áudio enquanto o aplicativo não está em primeiro plano. A UWP usa o modelo de aplicativo da Loja do Windows Phone; consulte [Áudio em segundo plano](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio) e o exemplo [Áudio em segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundAudio).

## <a name="cloud-services-networking-and-databases"></a>Serviços de nuvem, redes e bancos de dados

Hospedar serviços de aplicativos e dados na nuvem é possível com o Azure. Consulte [Introdução aos Serviços Móveis](https://azure.microsoft.com/documentation/articles/mobile-services-windows-store-get-started/). Para soluções que exigem dados online e offline, consulte: [Usando a sincronização de dados offline em Serviços Móveis](https://azure.microsoft.com/documentation/articles/mobile-services-windows-store-dotnet-get-started-offline-data/).

A UWP tem suporte parcial para a classe **System.Net.HttpWebRequest**, mas a classe **System.Net.WebClient** não tem suporte. A alternativa recomendada e prospectiva é a classe [**Windows.Web.Http.HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118)) se você precisar que seu código seja portável para outras plataformas que deem suporte ao .NET). Essas APIs usam [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) para representar uma solicitação HTTP.

No momento, aplicativos UWP não incluem suporte interno para cenários de uso intenso de dados, como cenários de LOB (linha de negócios). No entanto, você pode usar o SQLite para serviços de banco de dados transacional local. Para obter mais informações, consulte [SQLite](https://marketplace.visualstudio.com/items?itemName=SQLiteDevelopmentTeam.SQLiteforUniversalWindowsPlatform).

Passe URIs absolutos, URIs não relativos, para tipos de Tempo de Execução do Windows. Consulte [Passando um URI para o Tempo de Execução do Windows](https://docs.microsoft.com/dotnet/standard/cross-platform/passing-a-uri-to-the-windows-runtime).

## <a name="launchers-and-choosers"></a>Iniciadores e seletores

Com os iniciadores e os seletores (encontrados no namespace **Microsoft. Phone. Tasks** ), um aplicativo Windows Phone do Silverlight pode interagir com o sistema operacional para executar operações comuns, como compor um email, escolher uma foto ou compartilhar determinados tipos de dados com outro aplicativo. Procure **Microsoft. Phone. Tasks** no tópico Windows Phone os [mapeamentos do Silverlight para o namespace do Windows 10 e de classe](wpsl-to-uwp-namespace-and-class-mappings.md) para localizar o tipo UWP equivalente. Eles variam de mecanismos semelhantes, chamados iniciadores e seletores, à implementação de um contrato para o compartilhamento de dados entre aplicativos.

Um Windows Phone aplicativo do Silverlight pode ser colocado em um estado inativo ou até mesmo marcado para exclusão ao usar, por exemplo, a tarefa seletor de fotos. Um aplicativo UWP permanece ativo e em execução enquanto usa a classe [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker).

## <a name="monetization-trial-mode-and-in-app-purchases"></a>Monetização (modo de avaliação e compras no aplicativo)

Um Windows Phone aplicativo do Silverlight pode usar a classe UWP [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) para a maioria de seu modo de avaliação e a funcionalidade de compra no aplicativo, para que o código não precise ser portado. No entanto, um aplicativo Windows Phone do Silverlight chama **MarketplaceDetailTask. show** para oferecer o aplicativo para compra:

```csharp
    private void Buy()
    {
        MarketplaceDetailTask marketplaceDetailTask = new MarketplaceDetailTask();
        marketplaceDetailTask.ContentType = MarketplaceContentType.Applications;
        marketplaceDetailTask.Show();
    }
```

Porte esse código para chamar o método  [**RequestAppPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) UWP:

```csharp
    private async void Buy()
    {
        await Windows.ApplicationModel.Store.CurrentApp.RequestAppPurchaseAsync(false);
    }
```

Se você tiver o código que simula a sua compra de aplicativos e recursos de compra no aplicativo para fins de teste, poderá portá-lo para usar a classe [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) em seu lugar.

## <a name="notifications-for-tile-or-toast-updates"></a>Notificações de atualizações de bloco ou notificação do sistema

As notificações são uma extensão do modelo de notificação por push para aplicativos Windows Phone Silverlight. Quando você receber uma notificação do Serviço de Notificação por Push do Windows (WNS), poderá expor as informações para a interface do usuário com uma atualização de bloco ou uma notificação do sistema. Para portabilidade do lado da interface do usuário dos recursos de notificação, consulte [Blocos e notificações do sistema](w8x-to-uwp-porting-xaml-and-ui.md).

Para obter mais detalhes sobre o uso de notificações em um aplicativo UWP, consulte [Enviando notificações do sistema](https://docs.microsoft.com/previous-versions/windows/apps/hh868266(v=win.10)).

Para obter informações e tutoriais sobre como usar blocos, notificações do sistema e faixas em um aplicativo do Tempo de Execução do Windows criado em C++, C# ou Visual Basic, consulte [Trabalhando com blocos, selos e notificações do sistema](https://docs.microsoft.com/previous-versions/windows/apps/hh868259(v=win.10)).

## <a name="storage-file-access"></a>Armazenamento (acesso a arquivos)

Windows Phone código do Silverlight que armazena as configurações do aplicativo como pares chave-valor no armazenamento isolado é facilmente portado. Aqui está um exemplo antes e depois, primeiro a Windows Phone versão do Silverlight:

```csharp
    var propertySet = IsolatedStorageSettings.ApplicationSettings;
    const string key = "favoriteAuthor";
    propertySet[key] = "Charles Dickens";
    propertySet.Save();
    string myFavoriteAuthor = propertySet.Contains(key) ? (string)propertySet[key] : "<none>";
```

E o equivalente da UWP:

```csharp
    var propertySet = Windows.Storage.ApplicationData.Current.LocalSettings.Values;
    const string key = "favoriteAuthor";
    propertySet[key] = "Charles Dickens";
    string myFavoriteAuthor = propertySet.ContainsKey(key) ? (string)propertySet[key] : "<none>";
```

Embora um subconjunto do namespace **Windows. Storage** esteja disponível para eles, muitos Windows Phone aplicativos do Silverlight executam a e/s de arquivo com a classe **IsolatedStorageFile** porque ela tem suporte por mais tempo. Supondo que o **IsolatedStorageFile** está sendo usado, veja um exemplo antes e depois de escrever e ler um arquivo, primeiro a Windows Phone versão do Silverlight:

```csharp
    const string filename = "FavoriteAuthor.txt";
    using (var store = IsolatedStorageFile.GetUserStoreForApplication())
    {
        using (var streamWriter = new StreamWriter(store.CreateFile(filename)))
        {
            streamWriter.Write("Charles Dickens");
        }
        using (var StreamReader = new StreamReader(store.OpenFile(filename, FileMode.Open, FileAccess.Read)))
        {
            string myFavoriteAuthor = StreamReader.ReadToEnd();
        }
    }
```

E a mesma funcionalidade usando a UWP:

```csharp
    const string filename = "FavoriteAuthor.txt";
    var store = Windows.Storage.ApplicationData.Current.LocalFolder;
    Windows.Storage.StorageFile file = await store.CreateFileAsync(filename, Windows.Storage.CreationCollisionOption.ReplaceExisting);
    await Windows.Storage.FileIO.WriteTextAsync(file, "Charles Dickens");
    file = await store.GetFileAsync(filename);
    string myFavoriteAuthor = await Windows.Storage.FileIO.ReadTextAsync(file);
```

Um aplicativo Windows Phone Silverlight tem acesso somente leitura ao cartão SD opcional. Um aplicativo UWP tem acesso de leitura-gravação ao cartão SD. Para saber mais, consulte [Acessar o cartão SD](https://docs.microsoft.com/windows/uwp/files/access-the-sd-card).

Para obter informações sobre como acessar fotos, músicas e arquivos de vídeo em um aplicativo UWP, consulte [Arquivos e pastas nas bibliotecas Música, Fotos e Vídeos](https://docs.microsoft.com/windows/uwp/files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries).

Para obter mais informações, consulte [Arquivos, pastas e bibliotecas](https://docs.microsoft.com/windows/uwp/files/index).

O próximo tópico é [Portabilidade para o fator forma e a experiência do usuário](wpsl-to-uwp-form-factors-and-ux.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Namespace e mapeamentos de classe](wpsl-to-uwp-namespace-and-class-mappings.md)
 

