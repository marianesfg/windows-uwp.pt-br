---
description: Por trás de sua interface do usuário estão as camadas de negócios e dados.
title: Porting Windows Phone Silverlight business and data layers to UWP
ms.assetid: 27c66759-2b35-41f5-9f7a-ceb97f4a0e3f
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 25d8bba5e1b26613185017642d63128cc2b1f7f6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259087"
---
#  <a name="porting-windowsphone-silverlight-business-and-data-layers-to-uwp"></a>Porting Windows Phone Silverlight business and data layers to UWP


O tópico anterior foi [Portabilidade para E/S, dispositivo e modelo de aplicativo](wpsl-to-uwp-input-and-sensors.md).

Por trás de sua interface do usuário estão as camadas de negócios e dados. O código dessas camadas chama as APIs do sistema operacional e do .NET Framework (por exemplo, processamento em segundo plano, localização, a câmera, o sistema de arquivos, rede e outros acessos a dados). A maioria delas está [disponível para um aplicativo da Plataforma Universal do Windows (UWP)](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10)), portanto, espere poder fazer a portabilidade de grande parte desse código sem alteração.

## <a name="asynchronous-methods"></a>Métodos assíncronos

Uma das prioridades da Plataforma Universal do Windows (UWP) é habilitar você para criar aplicativos que sejam real e consistentemente responsivos. As animações são sempre suaves e as interações de toque, tais como movimento panorâmico e passagem do dedo, são instantâneas e sem latência, fazendo parecer que a interface do usuário está colada ao seu dedo. Para conseguir isso, qualquer API da UWP que não possa garantir a conclusão de um processo dentro de 50 ms se torna assíncrona e seu nome recebe o sufixo **Async**. Seu thread de interface do usuário retornará imediatamente após a chamada de um método **Async** e o trabalho será realizado em outro thread. Tornou-se muito fácil consumir um método **Async**, sintaticamente, usando o operador C# **await**, objetos de promessa JavaScript e continuações C++. (Para obter mais informações, consulte [Programação assíncrona](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps).)

## <a name="background-processing"></a>Processamento em segundo plano

A Windows Phone Silverlight app can use a managed **ScheduledTaskAgent** object to perform a task while the app is not in the foreground. Um aplicativo UWP usa a classe [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) para criar e registrar uma tarefa em segundo plano de maneira semelhante. Você define uma classe que implementa o trabalho de sua tarefa em segundo plano. O sistema executa sua tarefa em segundo plano periodicamente, chamando o método [**Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) da classe para executar o trabalho. Em um aplicativo UWP, lembre-se de definir a declaração **Tarefas em Segundo Plano** no manifesto do pacote do aplicativo. Para obter mais informações, consulte [Ofereça suporte ao seu aplicativo com tarefas em segundo plano](https://docs.microsoft.com/windows/uwp/launch-resume/support-your-app-with-background-tasks).

To transfer large data files in the background, a Windows Phone Silverlight app uses the **BackgroundTransferService** class. Um aplicativo UWP usa APIs no namespace [**Windows.Networking.BackgroundTransfer**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer) para fazer isso. Os recursos usam um padrão semelhante para iniciar transferências, mas a nova API melhorou as funcionalidades e o desempenho. Para obter mais informações, consulte [Transferindo dados em segundo plano](https://docs.microsoft.com/previous-versions/windows/apps/hh452975(v=win.10)).

A Windows Phone Silverlight app uses the managed classes in the **Microsoft.Phone.BackgroundAudio** namespace to play audio while the app is not in the foreground. A UWP usa o modelo de aplicativo da Loja do Windows Phone; consulte [Áudio em segundo plano](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio) e o exemplo [Áudio em segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundAudio).

## <a name="cloud-services-networking-and-databases"></a>Serviços de nuvem, redes e bancos de dados

Hospedar serviços de aplicativos e dados na nuvem é possível com o Azure. Consulte [Introdução aos Serviços Móveis](https://azure.microsoft.com/documentation/articles/mobile-services-windows-store-get-started/). Para soluções que exigem dados online e offline, consulte: [Usando a sincronização de dados offline em Serviços Móveis](https://azure.microsoft.com/documentation/articles/mobile-services-windows-store-dotnet-get-started-offline-data/).

A UWP tem suporte parcial para a classe **System.Net.HttpWebRequest**, mas a classe **System.Net.WebClient** não tem suporte. A alternativa recomendada e prospectiva é a classe [**Windows.Web.Http.HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118)) se você precisar que seu código seja portável para outras plataformas que deem suporte ao .NET). Essas APIs usam [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) para representar uma solicitação HTTP.

No momento, aplicativos UWP não incluem suporte interno para cenários de uso intenso de dados, como cenários de LOB (linha de negócios). No entanto, você pode usar o SQLite para serviços de banco de dados transacional local. Para obter mais informações, consulte [SQLite](https://marketplace.visualstudio.com/items?itemName=SQLiteDevelopmentTeam.SQLiteforUniversalWindowsPlatform).

Passe URIs absolutos, URIs não relativos, para tipos de Tempo de Execução do Windows. Consulte [Passando um URI para o Tempo de Execução do Windows](https://docs.microsoft.com/dotnet/standard/cross-platform/passing-a-uri-to-the-windows-runtime).

## <a name="launchers-and-choosers"></a>Iniciadores e seletores

With Launchers and Choosers (found in the **Microsoft.Phone.Tasks** namespace), a Windows Phone Silverlight app can interact with the operating system to perform common operations such as composing an email, choosing a photo, or sharing certain kinds of data with another app. Search for **Microsoft.Phone.Tasks** in the topic [Windows Phone Silverlight to Windows 10 namespace and class mappings](wpsl-to-uwp-namespace-and-class-mappings.md) to find the equivalent UWP type. Eles variam de mecanismos semelhantes, chamados iniciadores e seletores, à implementação de um contrato para o compartilhamento de dados entre aplicativos.

A Windows Phone Silverlight app can be put into a dormant state or even tombstoned when using, for example, the photo Chooser task. Um aplicativo UWP permanece ativo e em execução enquanto usa a classe [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker).

## <a name="monetization-trial-mode-and-in-app-purchases"></a>Monetização (modo de avaliação e compras no aplicativo)

A Windows Phone Silverlight app can use the UWP [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) class for most of its trial mode and in-app purchase functionality, so that code doesn't need to be ported. But, a Windows Phone Silverlight app calls **MarketplaceDetailTask.Show** to offer the app for purchase:

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

Notifications are an extension of the push notification model for Windows Phone Silverlight apps. Quando você receber uma notificação do Serviço de Notificação por Push do Windows (WNS), poderá expor as informações para a interface do usuário com uma atualização de bloco ou uma notificação do sistema. Para portabilidade do lado da interface do usuário dos recursos de notificação, consulte [Blocos e notificações do sistema](w8x-to-uwp-porting-xaml-and-ui.md).

Para obter mais detalhes sobre o uso de notificações em um aplicativo UWP, consulte [Enviando notificações do sistema](https://docs.microsoft.com/previous-versions/windows/apps/hh868266(v=win.10)).

Para obter informações e tutoriais sobre como usar blocos, notificações do sistema e faixas em um aplicativo do Tempo de Execução do Windows criado em C++, C# ou Visual Basic, consulte [Trabalhando com blocos, selos e notificações do sistema](https://docs.microsoft.com/previous-versions/windows/apps/hh868259(v=win.10)).

## <a name="storage-file-access"></a>Armazenamento (acesso a arquivos)

Windows Phone Silverlight code that stores app settings as key-value pairs in isolated storage is easily ported. Here is a before-and-after example, first the Windows Phone Silverlight version:

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

Although a subset of the **Windows.Storage** namespace is available to them, many Windows Phone Silverlight apps perform file i/o with the **IsolatedStorageFile** class because it has been supported for longer. Assuming that **IsolatedStorageFile** is being used, here's a before-and-after example of writing and reading a file, first the Windows Phone Silverlight version:

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

A Windows Phone Silverlight app has read-only access to the optional SD card. Um aplicativo UWP tem acesso de leitura-gravação ao cartão SD. Para saber mais, consulte [Acessar o cartão SD](https://docs.microsoft.com/windows/uwp/files/access-the-sd-card).

Para obter informações sobre como acessar fotos, músicas e arquivos de vídeo em um aplicativo UWP, consulte [Arquivos e pastas nas bibliotecas Música, Fotos e Vídeos](https://docs.microsoft.com/windows/uwp/files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries).

Para obter mais informações, consulte [Arquivos, pastas e bibliotecas](https://docs.microsoft.com/windows/uwp/files/index).

O próximo tópico é [Portabilidade para o fator forma e a experiência do usuário](wpsl-to-uwp-form-factors-and-ux.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Namespace and class mappings](wpsl-to-uwp-namespace-and-class-mappings.md)
 

