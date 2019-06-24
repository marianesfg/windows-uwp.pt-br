---
description: Por trás de sua interface do usuário estão as camadas de negócios e dados.
title: Portabilidade de camadas de dados e de negócios do Windows Phone Silverlight para UWP
ms.assetid: 27c66759-2b35-41f5-9f7a-ceb97f4a0e3f
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 25e7fdcb4195dcc0dffed7657d41bd02bea8a5c2
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67322307"
---
#  <a name="porting-windowsphone-silverlight-business-and-data-layers-to-uwp"></a>Portabilidade de camadas de dados e de negócios do Windows Phone Silverlight para UWP


O tópico anterior foi [Portabilidade para E/S, dispositivo e modelo de aplicativo](wpsl-to-uwp-input-and-sensors.md).

Por trás de sua interface do usuário estão as camadas de negócios e dados. O código dessas camadas chama as APIs do sistema operacional e do .NET Framework (por exemplo, processamento em segundo plano, localização, a câmera, o sistema de arquivos, rede e outros acessos a dados). A maioria delas está [disponível para um aplicativo da Plataforma Universal do Windows (UWP)](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10)), portanto, espere poder fazer a portabilidade de grande parte desse código sem alteração.

## <a name="asynchronous-methods"></a>Métodos assíncronos

Uma das prioridades da Plataforma Universal do Windows (UWP) é habilitar você para criar aplicativos que sejam real e consistentemente responsivos. As animações são sempre suaves e as interações de toque, tais como movimento panorâmico e passagem do dedo, são instantâneas e sem latência, fazendo parecer que a interface do usuário está colada ao seu dedo. Para conseguir isso, qualquer API da UWP que não possa garantir a conclusão de um processo dentro de 50 ms se torna assíncrona e seu nome recebe o sufixo **Async**. Seu thread de interface do usuário retornará imediatamente após a chamada de um método **Async** e o trabalho será realizado em outro thread. Tornou-se muito fácil consumir um método **Async**, sintaticamente, usando o operador C# **await**, objetos de promessa JavaScript e continuações C++. (Para obter mais informações, consulte [Programação assíncrona](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps).)

## <a name="background-processing"></a>Processamento em segundo plano

Um aplicativo do Windows Phone Silverlight pode usar um gerenciado **ScheduledTaskAgent** objeto para executar uma tarefa enquanto o aplicativo não estiver em primeiro plano. Um aplicativo UWP usa a classe [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) para criar e registrar uma tarefa em segundo plano de maneira semelhante. Você define uma classe que implementa o trabalho de sua tarefa em segundo plano. O sistema executa sua tarefa em segundo plano periodicamente, chamando o método [**Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) da classe para executar o trabalho. Em um aplicativo UWP, lembre-se de definir a declaração **Tarefas em Segundo Plano** no manifesto do pacote do aplicativo. Para obter mais informações, consulte [Ofereça suporte ao seu aplicativo com tarefas em segundo plano](https://docs.microsoft.com/windows/uwp/launch-resume/support-your-app-with-background-tasks).

Para transferir arquivos de dados grandes em segundo plano, um aplicativo do Windows Phone Silverlight usa o **BackgroundTransferService** classe. Um aplicativo UWP usa APIs no namespace [**Windows.Networking.BackgroundTransfer**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer) para fazer isso. Os recursos usam um padrão semelhante para iniciar transferências, mas a nova API melhorou as funcionalidades e o desempenho. Para obter mais informações, consulte [Transferindo dados em segundo plano](https://docs.microsoft.com/previous-versions/windows/apps/hh452975(v=win.10)).

Um aplicativo do Windows Phone Silverlight usa as classes gerenciadas do **backgroundaudio** namespace reproduzir áudio enquanto o aplicativo não está em primeiro plano. A UWP usa o modelo de aplicativo da Loja do Windows Phone; consulte [Áudio em segundo plano](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio) e o exemplo [Áudio em segundo plano](https://go.microsoft.com/fwlink/p/?linkid=619997).

## <a name="cloud-services-networking-and-databases"></a>Serviços de nuvem, redes e bancos de dados

Hospedar serviços de aplicativos e dados na nuvem é possível com o Azure. Consulte [Introdução aos Serviços Móveis](https://go.microsoft.com/fwlink/p/?LinkID=403138). Para soluções que exigem dados online e offline, consulte: [Usando a sincronização de dados offline nos serviços móveis](https://azure.microsoft.com/documentation/articles/mobile-services-windows-store-dotnet-get-started-offline-data/).

A UWP tem suporte parcial para a classe **System.Net.HttpWebRequest**, mas a classe **System.Net.WebClient** não tem suporte. A alternativa recomendada e prospectiva é a classe [**Windows.Web.Http.HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118)) se você precisar que seu código seja portável para outras plataformas que deem suporte ao .NET). Essas APIs usam [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) para representar uma solicitação HTTP.

No momento, aplicativos UWP não incluem suporte interno para cenários de uso intenso de dados, como cenários de LOB (linha de negócios). No entanto, você pode usar o SQLite para serviços de banco de dados transacional local. Para obter mais informações, consulte [SQLite](https://marketplace.visualstudio.com/items?itemName=SQLiteDevelopmentTeam.SQLiteforUniversalWindowsPlatform).

Passe URIs absolutos, URIs não relativos, para tipos de Tempo de Execução do Windows. Consulte [Passando um URI para o Tempo de Execução do Windows](https://docs.microsoft.com/dotnet/standard/cross-platform/passing-a-uri-to-the-windows-runtime).

## <a name="launchers-and-choosers"></a>Iniciadores e seletores

Com os iniciadores e seletores (encontrada na **encontram** namespace), um aplicativo do Windows Phone Silverlight pode interagir com o sistema operacional para executar operações comuns, como compor um email, escolhendo uma foto ou compartilhamento de determinados tipos de dados com outro aplicativo. Pesquise **encontram** no tópico [Windows Phone Silverlight para Windows 10 mapeamentos de namespace e classe](wpsl-to-uwp-namespace-and-class-mappings.md) para localizar o tipo equivalente do UWP. Eles variam de mecanismos semelhantes, chamados iniciadores e seletores, à implementação de um contrato para o compartilhamento de dados entre aplicativos.

Um aplicativo do Windows Phone Silverlight pode ser colocado em um estado inativo ou marcado para exclusão até mesmo ao usar, por exemplo, a tarefa de seletor de fotos. Um aplicativo UWP permanece ativo e em execução enquanto usa a classe [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker).

## <a name="monetization-trial-mode-and-in-app-purchases"></a>Monetização (modo de avaliação e compras no aplicativo)

Um aplicativo do Windows Phone Silverlight pode usar a UWP [**CurrentApp** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) de classe para a maior parte do seu modo de avaliação e no aplicativo de compra funcionalidade, para que o código não precisa ser movido. Mas, chama um aplicativo do Windows Phone Silverlight **MarketplaceDetailTask.Show** para o aplicativo de compra da oferta:

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

As notificações são uma extensão do modelo de notificação por push para aplicativos do Windows Phone Silverlight. Quando você receber uma notificação do Serviço de Notificação por Push do Windows (WNS), poderá expor as informações para a interface do usuário com uma atualização de bloco ou uma notificação do sistema. Para portabilidade do lado da interface do usuário dos recursos de notificação, consulte [Blocos e notificações do sistema](w8x-to-uwp-porting-xaml-and-ui.md).

Para obter mais detalhes sobre o uso de notificações em um aplicativo UWP, consulte [Enviando notificações do sistema](https://docs.microsoft.com/previous-versions/windows/apps/hh868266(v=win.10)).

Para obter informações e tutoriais sobre como usar blocos, notificações do sistema e faixas em um aplicativo do Tempo de Execução do Windows criado em C++, C# ou Visual Basic, consulte [Trabalhando com blocos, selos e notificações do sistema](https://docs.microsoft.com/previous-versions/windows/apps/hh868259(v=win.10)).

## <a name="storage-file-access"></a>Armazenamento (acesso a arquivos)

Código do Windows Phone Silverlight que armazena as configurações do aplicativo como pares chave-valor no armazenamento isolado é facilmente transferido. Aqui está um exemplo de antes e depois, primeiro a versão do Windows Phone Silverlight:

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

Embora seja um subconjunto do **Storage** namespace está disponível para eles, muitas Silverlight do Windows Phone executam aplicativos de arquivos e/s com o **IsolatedStorageFile** classe porque ele tem sido suportado para maior. Supondo que **IsolatedStorageFile** está sendo usado, aqui está um exemplo de antes e depois de gravar e ler um arquivo, primeiro a versão do Windows Phone Silverlight:

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

Um aplicativo do Windows Phone Silverlight tem acesso somente leitura para o cartão SD opcional. Um aplicativo UWP tem acesso de leitura-gravação ao cartão SD. Para saber mais, consulte [Acessar o cartão SD](https://docs.microsoft.com/windows/uwp/files/access-the-sd-card).

Para obter informações sobre como acessar fotos, músicas e arquivos de vídeo em um aplicativo UWP, consulte [Arquivos e pastas nas bibliotecas Música, Fotos e Vídeos](https://docs.microsoft.com/windows/uwp/files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries).

Para obter mais informações, consulte [Arquivos, pastas e bibliotecas](https://docs.microsoft.com/windows/uwp/files/index).

O próximo tópico é [Portabilidade para o fator forma e a experiência do usuário](wpsl-to-uwp-form-factors-and-ux.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Mapeamentos de Namespace e classe](wpsl-to-uwp-namespace-and-class-mappings.md)
 

