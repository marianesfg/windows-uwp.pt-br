---
author: mcleblanc
description: "Por trás de sua interface do usuário estão as camadas de negócios e dados."
title: "Portando camadas de negócios e dados do Windows Phone Silverlight para UWP"
ms.assetid: 27c66759-2b35-41f5-9f7a-ceb97f4a0e3f
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 57288b13af0f2ff4f677e2425442b43f3e76d4d8

---

#  Portando camadas de negócios e dados do Windows Phone Silverlight para UWP

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

O tópico anterior foi [Portabilidade para E/S, dispositivo e modelo de aplicativo](wpsl-to-uwp-input-and-sensors.md).

Por trás de sua interface do usuário estão as camadas de negócios e dados. O código dessas camadas chama as APIs do sistema operacional e do .NET Framework (por exemplo, processamento em segundo plano, localização, a câmera, o sistema de arquivos, rede e outros acessos a dados). A maioria delas está [disponível para um aplicativo da Plataforma Universal do Windows (UWP)](https://msdn.microsoft.com/library/windows/apps/br211369), portanto, espere poder fazer a portabilidade de grande parte desse código sem alteração.

## Métodos assíncronos

Uma das prioridades da Plataforma Universal do Windows (UWP) é habilitar você para criar aplicativos que sejam real e consistentemente responsivos. As animações são sempre suaves e as interações de toque, tais como movimento panorâmico e passagem do dedo, são instantâneas e sem latência, fazendo parecer que a interface do usuário está colada ao seu dedo. Para conseguir isso, qualquer API da UWP que não possa garantir a conclusão de um processo dentro de 50 ms se torna assíncrona e seu nome recebe o sufixo **Async**. Seu thread de interface do usuário retornará imediatamente após a chamada de um método **Async** e o trabalho será realizado em outro thread. Tornou-se muito fácil consumir um método **Async**, sintaticamente, usando o operador C# **await**, objetos de promessa JavaScript e continuações C++. (Para obter mais informações, consulte [Programação assíncrona](https://msdn.microsoft.com/library/windows/apps/mt187335).)

## Processamento em segundo plano

Um aplicativo do Windows Phone Silverlight pode usar um objeto **ScheduledTaskAgent** gerenciado para realizar uma tarefa enquanto o aplicativo não está em primeiro plano. Um aplicativo UWP usa a classe [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) para criar e registrar uma tarefa em segundo plano de maneira semelhante. Você define uma classe que implementa o trabalho de sua tarefa em segundo plano. O sistema executa sua tarefa em segundo plano periodicamente, chamando o método [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) da classe para executar o trabalho. Em um aplicativo UWP, lembre-se de definir a declaração **Tarefas em Segundo Plano** no manifesto do pacote do aplicativo. Para obter mais informações, consulte [Ofereça suporte ao seu aplicativo com tarefas em segundo plano](https://msdn.microsoft.com/library/windows/apps/mt299103).

Para transferir grandes arquivos de dados em segundo plano, um aplicativo Windows Phone Silverlight usa a classe **BackgroundTransferService**. Um aplicativo UWP usa APIs no namespace [**Windows.Networking.BackgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) para fazer isso. Os recursos usam um padrão semelhante para iniciar transferências, mas a nova API melhorou as funcionalidades e o desempenho. Para obter mais informações, consulte [Transferindo dados em segundo plano](https://msdn.microsoft.com/library/windows/apps/xaml/hh452975).

Um aplicativo do Windows Phone Silverlight usa as classes gerenciadas no namespace **Microsoft.Phone.BackgroundAudio** para reproduzir áudio enquanto o aplicativo não está em primeiro plano. A UWP usa o modelo de aplicativo da Loja do Windows Phone; consulte [Áudio em segundo plano](https://msdn.microsoft.com/library/windows/apps/mt282140) e o exemplo [Áudio em segundo plano](http://go.microsoft.com/fwlink/p/?linkid=619997).

## Serviços de nuvem, redes e bancos de dados

Hospedar serviços de aplicativos e dados na nuvem é possível com o Azure. Consulte [Introdução aos Serviços Móveis](http://go.microsoft.com/fwlink/p/?LinkID=403138). Para soluções que exigem dados online e offline, consulte: [Usando a sincronização de dados offline em Serviços Móveis](http://azure.microsoft.com/documentation/articles/mobile-services-windows-store-dotnet-get-started-offline-data/).

A UWP tem suporte parcial para a classe **System.Net.HttpWebRequest**, mas a classe **System.Net.WebClient** não tem suporte. A alternativa recomendada e prospectiva é a classe [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx) se você precisar que seu código seja portável para outras plataformas que deem suporte ao .NET). Essas APIs usam [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) para representar uma solicitação HTTP.

No momento, aplicativos UWP não incluem suporte interno para cenários de uso intenso de dados, como cenários de LOB (linha de negócios). No entanto, você pode usar o SQLite para serviços de banco de dados transacional local. Para obter mais informações, consulte [SQLite](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936).

Passe URIs absolutos, URIs não relativos, para tipos de Tempo de Execução do Windows. Consulte [Passando um URI para o Tempo de Execução do Windows](https://msdn.microsoft.com/library/hh763341.aspx).

## Iniciadores e seletores

Com iniciadores e seletores (encontrados no namespace **Microsoft.Phone.Tasks**), um aplicativo do Windows Phone Silverlight pode interagir com o sistema operacional para executar operações comuns, como escrever um email, escolher uma foto ou compartilhar determinados tipos de dados com outro aplicativo. Procure por **Microsoft.Phone.Tasks** no tópico [Windows Phone Silverlight para mapeamentos de classe e namespace do Windows 10](wpsl-to-uwp-namespace-and-class-mappings.md) a fim de encontrar o tipo equivalente da UWP. Eles variam de mecanismos semelhantes, chamados iniciadores e seletores, à implementação de um contrato para o compartilhamento de dados entre aplicativos.

Um aplicativo Windows Phone Silverlight pode ser colocado em um estado inativo ou até mesmo marcado para exclusão ao usar, por exemplo, a tarefa de seletor de foto. Um aplicativo UWP permanece ativo e em execução enquanto usa a classe [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847).

## Monetização (modo de avaliação e compras no aplicativo)

Um aplicativo Windows Phone Silverlight pode usar a classe  [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) UWP para a maior parte da funcionalidade do modo de avaliação e de compra realizada em aplicativo, de maneira que o código não precise ser portado. Porém, um aplicativo do Windows Phone Silverlight chama **MarketplaceDetailTask.Show** para oferecer o aplicativo para compra:

```csharp
    private void Buy()
    {
        MarketplaceDetailTask marketplaceDetailTask = new MarketplaceDetailTask();
        marketplaceDetailTask.ContentType = MarketplaceContentType.Applications;
        marketplaceDetailTask.Show();
    }
```

Porte esse código para chamar o método  [**RequestAppPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/hh967813) UWP:

```csharp
    private async void Buy()
    {
        await Windows.ApplicationModel.Store.CurrentApp.RequestAppPurchaseAsync(false);
    }
```

Se você tiver o código que simula a sua compra de aplicativos e recursos de compra no aplicativo para fins de teste, poderá portá-lo para usar a classe [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) em seu lugar.

## Notificações de atualizações de bloco ou notificação do sistema

As notificações são uma extensão do modelo de notificação por push dos aplicativos Windows Phone Silverlight. Quando você receber uma notificação do Serviço de Notificação por Push do Windows (WNS), poderá expor as informações para a interface do usuário com uma atualização de bloco ou uma notificação do sistema. Para portabilidade do lado da interface do usuário dos recursos de notificação, consulte [Blocos e notificações do sistema](w8x-to-uwp-porting-xaml-and-ui.md#tiles-and-toasts).

Para obter mais detalhes sobre o uso de notificações em um aplicativo UWP, consulte [Enviando notificações do sistema](https://msdn.microsoft.com/library/windows/apps/xaml/hh868266).

Para obter informações e tutoriais sobre como usar blocos, notificações do sistema e faixas em um aplicativo do Tempo de Execução do Windows criado em C++, C# ou Visual Basic, consulte [Trabalhando com blocos, selos e notificações do sistema](https://msdn.microsoft.com/library/windows/apps/xaml/hh868259).

## Armazenamento (acesso a arquivos)

O código do Windows Phone Silverlight que armazena configurações do aplicativo como pares chave-valor no armazenamento isolado pode ser facilmente portado. Aqui está um exemplo de antes e depois, primeiro a versão do Windows Phone Silverlight:

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

Embora um subconjunto do namespace **Windows.Storage** esteja disponível para eles, muitos aplicativos Windows Phone Silverlight executam o processo de E/S de arquivos com a classe **IsolatedStorageFile** porque ela tem suporte há mais tempo. Supondo que **IsolatedStorageFile** esteja sendo usada, aqui está um exemplo de antes e depois da gravação e leitura de um arquivo, primeiro a versão do Windows Phone Silverlight:

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

Um aplicativo Windows Phone Silverlight tem acesso somente leitura ao cartão SD opcional. Um aplicativo UWP tem acesso de leitura-gravação ao cartão SD. Para saber mais, consulte [Acessar o cartão SD](https://msdn.microsoft.com/library/windows/apps/mt188699).

Para obter informações sobre como acessar fotos, músicas e arquivos de vídeo em um aplicativo UWP, consulte [Arquivos e pastas nas bibliotecas Música, Fotos e Vídeos](https://msdn.microsoft.com/library/windows/apps/mt188703).

Para obter mais informações, consulte [Arquivos, pastas e bibliotecas](https://msdn.microsoft.com/library/windows/apps/mt185399).

O próximo tópico é [Portabilidade para o fator forma e a experiência do usuário](wpsl-to-uwp-form-factors-and-ux.md).

## Tópicos relacionados

* [Mapeamentos de namespace e de classe](wpsl-to-uwp-namespace-and-class-mappings.md)
 




<!--HONumber=Aug16_HO3-->


