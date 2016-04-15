---
description: use a API de transferência em segundo plano para copiar arquivos de maneira confiável na rede.
title: transferências em segundo plano
ms.assetid: 1207B089-BC16-4BF0-BBD4-FD99950C764B
---

# Transferências em segundo plano

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**APIs Importantes**

-   [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242)
-   [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998)
-   [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)

Use a API de transferência em segundo plano para copiar arquivos de maneira confiável na rede. A API de transferência em segundo plano fornece recursos avançados de carregamento e download que são executados em segundo plano durante a suspensão do aplicativo e persistirão após o encerramento do aplicativo. A API monitora o status da rede e automaticamente suspende e retoma transferências quando a conexão é perdida. As transferências também reconhecem o sensor de dados e de bateria, ou seja, a atividade de download se ajusta de acordo com a conectividade atual e o status de bateria do dispositivo. A API é ideal para carregar e baixar arquivos muito grandes usando HTTP(S). Também há suporte a FTP, mas apenas para downloads.

A transferência em segundo plano é executada separadamente do aplicativo de chamada e foi criada, principalmente, para operações de transferência de longo prazo para recursos como vídeo, música e imagens grandes. Para essas situações, é essencial usar a transferência em segundo plano, pois os downloads continuam progredindo mesmo quando o aplicativo é suspenso.

Se você for baixar recursos pequenos que, provavelmente, serão completos com rapidez, é melhor usar as APIs [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) ao invés da transferência em segundo plano.

## Usando Windows.Networking.BackgroundTransfer


### Como o recurso Transferência em Segundo Plano funciona?

Quando um aplicativo usa transferência em segundo plano para iniciar uma transferência, a solicitação é configurada e inicializada usando os objetos de classe [**BackgroundDownloader**](https://msdn.microsoft.com/library/windows/apps/br207126) ou [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140). Cada operação de transferência é manipulada individualmente pelo sistema e separada do aplicativo de chamada. As informações de andamento estão disponíveis se você deseja fornecer ao usuário status na interface de usuário do aplicativo, e seu aplicativo pode pausar, retomar, cancelar ou mesmo ler os dados enquanto ocorre a transferência. A maneira como as transferências são manipuladas pelo sistema promove o uso inteligente de energia e evita problemas que poderiam surgir quando um aplicativo conectado encontra eventos como suspensão e finalização de aplicativo ou alterações repentinas no status de rede.

### Executando solicitações de arquivos autenticados com a transferência em segundo plano

A transferência em segundo plano proporciona métodos que oferecem suporte para credenciais básicas de servidor e proxy, cookies e uso de cabeçalhos HTTP personalizados (via [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146)) para cada operação de transferência.

### Como esse recurso se adapta a alterações no status de rede ou desligamentos inesperados?

O recurso de transferência em segundo plano mantém uma experiência consistente para cada operação de transferência quando ocorrem alterações no status de rede, impulsionando de modo inteligente a conectividade e as informações do status do plano de dados da operadora fornecidas pelo recurso de [Conectividade](https://msdn.microsoft.com/library/windows/apps/hh452990). Para definir comportamentos para diferentes situações de rede, um aplicativo configura uma política de custo para cada operação usando valores definidos pela [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138).

Por exemplo, a política de custo definida para uma operação pode indicar que a operação deve ser pausada automaticamente quando o dispositivo estiver usando uma rede limitada. A transferência é, então, retomada (ou reiniciada) automaticamente quando uma conexão a uma rede "não restrita" é estabelecida. Para obter mais informações sobre como as redes são definidas por custo, consulte [**NetworkCostType**](https://msdn.microsoft.com/library/windows/apps/br207292).

Embora o recurso de transferência em segundo plano tenha seus próprios mecanismos para manipular alterações no status de rede, existem outras considerações gerais de conectividade para aplicativos conectados a rede. Leia sobre o [aproveitamento de informações de conexão de rede disponíveis](https://msdn.microsoft.com/library/windows/apps/hh452983) para saber mais.

> **Nota**  Para aplicativos sendo executados em dispositivos móveis, existem recursos que permitem ao usuário monitorar e restringir a quantidade de dados que é transferida com base no tipo de conexão, no status de roaming e no plano de dados do usuário. Por isso, as transferências em segundo plano podem ser pausadas no telefone mesmo quando a [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) indica que a transferência deve continuar.

A tabela a seguir indica quando as transferências em segundo plano são permitidas no telefone para cada valor da [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138), levando em consideração o status atual do telefone. Você pode usar a classe [**ConnectionCost**](https://msdn.microsoft.com/library/windows/apps/br207244) para determinar o status atual do telefone.

| Status do dispositivo                                                                                                                      | UnrestrictedOnly | Padrão | Sempre |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------|---------|--------|
| Conectado ao WiFi                                                                                                                 | Permitido            | Permitido   | Permitido  |
| Conexão limitada, sem roaming, sob limite de dados, sob controle para permanecer dentro do limite                                                   | Negado             | Permitido   | Permitido  |
| Conexão limitada, sem roaming, sob limite de dados, sob controle para exceder o limite                                                       | Negado             | Negado    | Permitido  |
| Conexão limitada, roaming, sob limite de dados                                                                                     | Negado             | Negado    | Permitido  |
| Conexão limitada, acima do limite de dados. Esse estado ocorre somente quando o usuário habilita a opção "Restringir dados em segundo plano na interface do usuário de Data Sense". | Negado             | Negado    | Negado   |

 

## Carregando arquivos


Quando a transferência em segundo plano é usada, um carregamento existe como uma [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) que expõe uma quantidade de métodos de controle usados para reiniciar ou cancelar a operação. Eventos de aplicativo (ex.: suspensão ou finalização) e alterações de conectividade são manipulados automaticamente pelo sistema por **UploadOperation**. Os uploads continuarão durante os períodos de suspensão ou pausas do aplicativo e prosseguirão depois da finalização do aplicativo. Além disso, a configuração da propriedade [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018) indicará se seu aplicativo iniciará uploads quando uma rede limitada estiver sendo usada para conectividade com a Internet ou não.

Os exemplos a seguir orientarão você na criação e inicialização de um upload básico e mostrarão como enumerar e reintroduzir operações que persistiram desde a sessão anterior do aplicativo.

### Carregando um único arquivo

A criação de um upload começa com [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140). Essa classe é usada para fornecer os métodos que capacitam seu aplicativo a configurar o upload antes de criar a [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) resultante. O exemplo a seguir mostra como fazer isso com os objetos [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) e [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) exigidos.

**Identifique o arquivo e o destino para o upload**

Antes de podermos começar com a criação de uma [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), primeiro precisamos identificar a URI do local para o qual carregar e o arquivo que será carregado. No exemplo a seguir, o valor *uriString* é preenchido usando uma cadeia de caracteres da entrada de interface do usuário e o valor *file* usando o objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) retornado por uma operação [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275).

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_B "Identificar o arquivo e o destino para o upload")]

**Crie e inicialize a operação de upload**

Na etapa anterior, os valores *uriString* e *file* são passados para uma instância de nosso próximo exemplo, UploadOp, em que eles são usados para configurar e iniciar a nova operação de upload. Primeiro, *uriString* é analisada para criar o objeto [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998).

Em seguida, as propriedades do [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) (*file*) fornecido são usadas por [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) para popular o cabeçalho de solicitação e configurar a propriedade *SourceFile* com o objeto **StorageFile**. O método [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146) é, então, chamado para inserir o nome do arquivo, fornecido como uma cadeia de caracteres, e a propriedade [**StorageFile.Name**](https://msdn.microsoft.com/library/windows/apps/br227220).

Por fim, [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) cria a [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) (*carregar*).

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_A "Criar e inicializar a operação de upload")]

Observe as chamadas de método assíncrono definidas usando promessas do JavaScript. Olhando para uma linha do último exemplo:

```javascript
promise = upload.startAsync().then(complete, error, progress);
```

    The async method call is followed by a then statement which indicates methods, defined by the app, that are called when a result from the async method call is returned. For more information on this programming pattern, see [Asynchronous programming in JavaScript using promises](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx).

### Carregando vários arquivos

**Identifique os arquivos e o destino para o upload**

    In a scenario involving multiple files transferred with a single [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), the process begins as it usually does by first providing the required destination URI and local file information. Similar to the example in the previous section, the URI is provided as a string by the end-user and [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) can be used to provide the ability to indicate files through the user interface as well. However, in this scenario the app should instead call the [**PickMultipleFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br207851) method to enable the selection of multiple files through the UI.

```javascript
function uploadFiles() {
       var filePicker = new Windows.Storage.Pickers.FileOpenPicker();
       filePicker.fileTypeFilter.replaceAll(["*"]);

       filePicker.pickMultipleFilesAsync().then(function (files) {
          if (files === 0) {
             printLog("No file selected");
                return;
          }

          var upload = new UploadOperation();
          var uriString = document.getElementById("serverAddressField").value;
          upload.startMultipart(uriString, files);

          // Persist the upload operation in the global array.
          uploadOperations.push(upload);
       });
    }
```

**Crie objetos para os parâmetros fornecidos**

    The next two examples use code contained in a single example method, **startMultipart**, which was called at the end of the last step. For the purpose of instruction the code in the method that creates an array of [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) objects has been split from the code that creates the resultant [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224).

    First, the URI string provided by the user is initialized as a [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998). Next, the array of [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) objects (**files**) passed to this method is iterated through, each object is used to create a new [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) object which is then placed in the **contentParts** array.

```javascript
upload.startMultipart = function (uriString, files) {
        try {
            var uri = new Windows.Foundation.Uri(uriString);
            var uploader = new Windows.Networking.BackgroundTransfer.BackgroundUploader();

            var contentParts = [];
            files.forEach(function (file, index) {
                var part = new Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart("File" + index, file.name);
                part.setFile(file);
                contentParts.push(part);
            });
```

**Crie e inicialize a operação de upload em várias partes**

    With our contentParts array populated with all of the [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) objects representing each [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) for upload, we are ready to call [**CreateUploadAsync**](https://msdn.microsoft.com/library/windows/apps/hh923973) using the [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) to indicate where the request will be sent.

```javascript
        // Create a new upload operation.
            uploader.createUploadAsync(uri, contentParts).then(function (uploadOperation) {

               // Start the upload and persist the promise to be able to cancel the upload.
               upload = uploadOperation;
               promise = uploadOperation.startAsync().then(complete, error, progress);
            });

         } catch (err) {
             displayError(err);
         }
     };
```

### Reiniciando operações de upload interrompidas

Quaisquer recursos de sistema associados são liberados na conclusão ou no cancelamento de uma [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224). Contudo, se seu aplicativo for finalizado antes disso acontecer, quaisquer operações ativas serão pausadas e os recursos associados a cada uma permanecerão ocupados. Se as operações não forem enumeradas e reintroduzidas na próxima sessão de aplicativo, elas não serão concluídas e continuarão ocupando os recursos do dispositivo.

1.  Antes de definir a função que enumera operações persistidas, precisamos criar uma matriz que contenha os objetos [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) que retornarão:

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_C "Reiniciar operação de upload interrompida")]

2.  Em seguida, definimos a função que enumera operações persistidas e as armazena em nossa matriz. Observe que o método **load** chamado para reatribuir revogações à [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), caso persista com a finalização do aplicativo, está na classe UploadOp que definimos mais tarde nesta seção.

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_D "Enumerar operações persistidas")]

## Baixando arquivos

Quando a transferência em segundo plano é usada, cada download existe como uma [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) que expõe uma quantidade de métodos de controle usados para pausar, retomar, reiniciar e cancelar a operação. Eventos de aplicativo (ex.: suspensão ou finalização) e alterações de conectividade são manipulados automaticamente pelo sistema por **DownloadOperation**. Os downloads continuarão durante os períodos de suspensão ou pausas do aplicativo e prosseguirão depois da finalização do aplicativo. Para situações de rede móvel, a configuração da propriedade [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018) indicará se seu aplicativo iniciará ou continuará downloads quando uma rede limitada estiver sendo usada para conectividade com a Internet ou não.

Se você for baixar recursos pequenos que, provavelmente, serão completos com rapidez, é melhor usar as APIs [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) ao invés da transferência em segundo plano.

Os exemplos a seguir o guiarão através da criação e da inicialização de um download básico e como enumerar e reintroduzir operações persistidas de uma sessão de aplicativo anterior.

### Configurar e iniciar um download de arquivo de Transferência de Tela de Fundo

O exemplo a seguir demonstra como cadeias de caracteres representando uma URI e um nome de arquivo podem ser usadas para criar um objeto [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) e o [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que conterá o arquivo solicitado. Neste exemplo, o novo arquivo é colocado automaticamente em um local pré-definido. Como alternativa, [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) pode ser usado para permitir que os usuários indiquem onde salvar o arquivo no dispositivo. Observe que o método **load** chamado para reatribuir revogações à [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154), caso persista com a finalização do aplicativo, está na classe DownloadOp definida mais tarde nesta seção.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_A)]

Observe as chamadas de método assíncrono definidas usando promessas do JavaScript. Olhando para a linha 17 do exemplo de código anterior:

```javascript
promise = download.startAsync().then(complete, error, progress);
```

A chamada de método assíncrono é seguida por uma instrução then que indica métodos, definidos pelo aplicativo, que são chamados quando é retornado um resultado da chamada de método assíncrono. Para obter mais informações sobre esse padrão de programação, consulte [Programação assíncrona em JavaScript usando promessas](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx).

### Adicionando outros métodos de controle de operação

O nível de controle pode ser aumentado com a implementação de métodos [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) adicionais. Por exemplo, adicionar o seguinte código ao exemplo acima introduzirá a habilidade de cancelar o download.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_B)]

### Enumerando operações persistidas na inicialização

Quaisquer recursos de sistema associados são liberados na conclusão ou no cancelamento de uma [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154). Contudo, se seu aplicativo for finalizado antes da ocorrência de algum desses eventos, os downloads serão pausados e persistirão em segundo plano. Os exemplos a seguir demonstram como reintroduzir downloads persistidos em uma nova sessão de aplicativo.

1.  Antes de definir a função que enumera operações persistidas, precisamos criar uma matriz que contenha os objetos [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) que retornarão:

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_D)]

2.  Em seguida, definimos a função que enumera operações persistidas e as armazena em nossa matriz. Observe que o método **load** chamado para reatribuir revogações para uma [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) persistida está no exemplo de DownloadOp que definimos mais tarde nesta seção.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_E)]

3.  Agora você pode usar a lista preenchida para reiniciar operações pendentes.

## Pós-processamento

Um novo recurso no Windows 10 é a capacidade de executar códigos de aplicativo na conclusão de uma transferência em segundo plano, mesmo quando o aplicativo não estiver sendo executado. Por exemplo, seu aplicativo pode atualizar uma lista de filmes disponíveis após o download de um filme, em vez de fazer seu aplicativo procurar novos filmes sempre que ele é iniciado. Ou, seu aplicativo pode manipular uma transferência de arquivo com falha por tentar novamente usar um servidor ou porta diferente. O pós-processamento é invocado para transferências com êxito e falhas, de modo que pode usá-lo para implementar manipulação de erro personalizada e lógica de repetição.

O pós-processamento usa a infraestrutura da tarefa em segundo plano existente. Crie uma tarefa em segundo plano e associe-a a suas transferências antes de iniciar as transferências. As transferências são executadas em segundo plano e, quando são concluídas, sua tarefa em segundo plano é chamada para ser executada pós-processamento.

O pós-processamento usa uma nova classe: [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209). Essa classe é similar a [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030) existente, no sentido de que permite que você agrupe as transferências em segundo plano, mas **BackgroundTransferCompletionGroup** adiciona a habilidade de designar uma tarefa em segundo plano a ser executada quando a transferência for concluída.

Você inicia uma transferência em segundo plano com pós-processamento da seguinte maneira.

1.  Crie um objeto [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209). Depois, crie um objeto [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Configure a propriedade **Trigger** do objeto construtor para o objeto de grupo de conclusão e a propriedade **TaskEngtyPoint** do construtor para o ponto de entrada da tarefa em segundo plano que deve ser executada na conclusão da transferência. Por fim, chame o método [**BackgroundTaskBuilder.Register**](https://msdn.microsoft.com/library/windows/apps/br224772) para registrar sua tarefa em segundo plano. Observe que vários grupos de conclusão podem compartilhar um ponto de entrada de tarefa em segundo plano, mas você pode ter apenas um grupo de conclusão por registro de tarefa em segundo plano.

   ```csharp
    var completionGroup = new BackgroundTransferCompletionGroup();
    BackgroundTaskBuilder builder = new BackgroundTaskBuilder();

    builder.Name = "MyDownloadProcessingTask";
    builder.SetTrigger(completionGroup.Trigger);
    builder.TaskEntryPoint = "Tasks.BackgroundDownloadProcessingTask";

    BackgroundTaskRegistration downloadProcessingTask = builder.Register();
    ```

2.  Next you associate background transfers with the completion group. Once all transfers are created, enable the completion group.

   ```csharp
    BackgroundDownloader downloader = new BackgroundDownloader(completionGroup);
    DownloadOperation download = downloader.CreateDownload(uri, file);
    Task<DownloadOperation> startTask = download.StartAsync().AsTask();

    // App still sees the normal completion path
    startTask.ContinueWith(ForegroundCompletionHandler);

    // Do not enable the CompletionGroup until after all downloads are created.
    downloader.CompletinGroup.Enable();
    ```

3.  The code in the background task extracts the list of operations from the trigger details, and your code can then inspect the details for each operation and perform appropriate post-processing for each operation.

   ```csharp
    public class BackgroundDownloadProcessingTask : IBackgroundTask
    {
      public async void Run(IBackgroundTaskInstance taskInstance)
      {
        var details = (BackgroundTransferCompletionGroupTriggerDetails)taskInstance.TriggerDetails;
        IReadOnlyList<DownloadOperation> downloads = details.Downloads;

        // Do post-processing on each finished operation in the list of downloads
      }
    }
    ```

The post-processing task is a regular background task. It is part of the pool of all background tasks, and it is subject to the same resource management policy as all background tasks.

Also, note that post-processing does not replace foreground completion handlers. If your app defines a foreground completion handler, and your app is running when the file transfer completes, then both your foreground completion handler and your background completion handler will be called. The order in which foreground and background tasks are called is not guaranteed. If you define both, you should ensure that the two tasks will work properly and not interfere with each other if they are running concurrently.

## Request timeouts

There are two primary connection timeout scenarios to take into consideration:

-   When establishing a new connection for a transfer, the connection request is aborted if it is not established within five minutes.

-   After a connection has been established, an HTTP request message that has not received a response within two minutes is aborted.

> **Note**  In either scenario, assuming there is Internet connectivity, Background Transfer will retry a request up to three times automatically. In the event Internet connectivity is not detected, additional requests will wait until it is.

## Debugging guidance

Stopping a debugging session in Microsoft Visual Studio is comparable to closing your app; PUT uploads are paused and POST uploads are terminated. Even while debugging, your app should enumerate and then restart or cancel any persisted uploads. For example, you can have your app cancel enumerated persisted upload operations at app startup if there is no interest in previous operations for that debug session.

While enumerating downloads/uploads on app startup during a debug session, you can have your app cancel them if there is no interest in previous operations for that debug session. Note that if there are Visual Studio project updates, like changes to the app manifest, and the app is uninstalled and re-deployed, [**GetCurrentUploadsAsync**](https://msdn.microsoft.com/library/windows/apps/hh701149) cannot enumerate operations created using the previous app deployment.

When using Background Transfer during development, you may get into a situation where the internal caches of active and completed transfer operations can get out of sync. This may result in the inability to start new transfer operations or interact with existing operations and [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030) objects. In some cases, attempting to interact with existing operations may trigger a crash. This result can occur if the [**TransferBehavior**](https://msdn.microsoft.com/library/windows/apps/dn279033) property is set to **Parallel**. This issue occurs only in certain scenarios during development and is not applicable to end users of your app.

Four scenarios using Visual Studio can cause this issue.

-   You create a new project with the same app name as an existing project, but a different language (from C++ to C#, for example).
-   You change the target architecture (from x86 to x64, for example) in an existing project.
-   You change the culture (from neutral to en-US, for example) in an existing project.
-   You add or remove a capability in the package manifest (adding **Enterprise Authentication**, for example) in an existing project.

Regular app servicing, including manifest updates which add or remove capabilities, do not trigger this issue on end user deployments of your app.
To work around this issue, completely uninstall all versions of the app and re-deploy with the new language, architecture, culture, or capability. This can be done via the **Start** screen or using PowerShell and the **Remove-AppxPackage** cmdlet.

## Exceptions in Windows.Networking.BackgroundTransfer

An exception is thrown when an invalid string for a the Uniform Resource Identifier (URI) is passed to the constructor for the [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) object.

**.NET:  **The [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) type appears as [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) in C# and VB.

In C# and Visual Basic, this error can be avoided by using the [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) class in the .NET 4.5 and one of the [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) methods to test the string received from the app user before the URI is constructed.

In C++, there is no method to try and parse a string to a URI. If an app gets input from the user for the [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998), the constructor should be in a try/catch block. If an exception is thrown, the app can notify the user and request a new hostname.

The [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) namespace has convenient helper methods and uses enumerations in the [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) namespace for handling errors. This can be useful for handling specific network exceptions differently in your app.

An error encountered on an asynchronous method in the [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) namespace is returned as an **HRESULT** value. The [**BackgroundTransferError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701093) method is used to convert a network error from a background transfer operation to a [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818) enumeration value. Most of the **WebErrorStatus** enumeration values correspond to an error returned by the native HTTP or FTP client operation. An app can filter on specific **WebErrorStatus** enumeration values to modify app behavior depending on the cause of the exception.

For parameter validation errors, an app can also use the **HRESULT** from the exception to learn more detailed information on the error that caused the exception. Possible **HRESULT** values are listed in the *Winerror.h* header file. For most parameter validation errors, the **HRESULT** returned is **E\_INVALIDARG**.



<!--HONumber=Mar16_HO1-->


