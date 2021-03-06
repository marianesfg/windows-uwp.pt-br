---
description: Use a API de transferência em segundo plano para copiar arquivos de maneira confiável na rede.
title: Transferências em segundo plano
ms.assetid: 1207B089-BC16-4BF0-BBD4-FD99950C764B
ms.date: 03/23/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d8a4c354eff34edb0c97e9d95828d4287f9c4b99
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "72282494"
---
# <a name="background-transfers"></a>Transferências em segundo plano
Use a API de transferência em segundo plano para copiar arquivos de maneira confiável na rede. A API de transferência em segundo plano fornece recursos avançados de upload e download que são executados em segundo plano durante a suspensão do aplicativo e persistem após o encerramento do aplicativo. A API monitora o status da rede e automaticamente suspende e retoma transferências quando a conexão é perdida. As transferências também reconhecem o sensor de dados e de bateria, ou seja, a atividade de download se ajusta de acordo com a conectividade atual e o status de bateria do dispositivo. A API é ideal para carregar e baixar arquivos muito grandes usando HTTP(S). Também há suporte a FTP, mas apenas para downloads.

A transferência em segundo plano é executada separadamente do aplicativo de chamada e foi criada, principalmente, para operações de transferência de longo prazo para recursos como vídeo, música e imagens grandes. Para essas situações, é essencial usar a transferência em segundo plano, pois os downloads continuam progredindo mesmo quando o aplicativo é suspenso.

Se você for baixar recursos pequenos que, provavelmente, serão completos com rapidez, é melhor usar as APIs [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) ao invés da transferência em segundo plano.

## <a name="using-windowsnetworkingbackgroundtransfer"></a>Usando Windows.Networking.BackgroundTransfer

### <a name="how-does-the-background-transfer-feature-work"></a>Como o recurso Transferência em Segundo Plano funciona?
Quando um aplicativo usa transferência em segundo plano para iniciar uma transferência, a solicitação é configurada e inicializada usando os objetos de classe [**BackgroundDownloader**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundDownloader) ou [**BackgroundUploader**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundUploader). Cada operação de transferência é manipulada individualmente pelo sistema e separada do aplicativo de chamada. As informações de andamento estão disponíveis se você deseja fornecer ao usuário status na interface de usuário do aplicativo, e seu aplicativo pode pausar, retomar, cancelar ou mesmo ler os dados enquanto ocorre a transferência. A maneira como as transferências são manipuladas pelo sistema promove o uso inteligente de energia e evita problemas que poderiam surgir quando um aplicativo conectado encontra eventos como suspensão e finalização de aplicativo ou alterações repentinas no status de rede.

> [!NOTE]
> Devido às restrições de recurso por aplicativo, um aplicativo não deve ter mais de 200 transferências (DownloadOperations + UploadOperations) de cada vez. Exceder esse limite pode deixar a fila de transferência do aplicativo em um estado irrecuperável.

Quando um aplicativo for iniciado, ele deverá chamar [**AttachAsync**](/uwp/api/windows.networking.backgroundtransfer.downloadoperation.AttachAsync) em todos os objetos [**DownloadOperation**](/uwp/api/windows.networking.backgroundtransfer.downloadoperation) e [**UploadOperation**](/uwp/api/windows.networking.backgroundtransfer.uploadoperation) existentes. Se você não fizer isso perderá as transferências já concluídas e acabará inutilizando seu uso do recurso de Transferência em Segundo Plano.

### <a name="performing-authenticated-file-requests-with-background-transfer"></a>Executando solicitações de arquivos autenticados com a transferência em segundo plano
A transferência em segundo plano proporciona métodos que oferecem suporte para credenciais básicas de servidor e proxy, cookies e uso de cabeçalhos HTTP personalizados (via [**SetRequestHeader**](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.backgrounduploader.setrequestheader)) para cada operação de transferência.

### <a name="how-does-this-feature-adapt-to-network-status-changes-or-unexpected-shutdowns"></a>Como esse recurso se adapta a alterações no status de rede ou desligamentos inesperados?
O recurso de transferência em segundo plano mantém uma experiência consistente para cada operação de transferência quando ocorrem alterações no status de rede, impulsionando de modo inteligente a conectividade e as informações do status do plano de dados da operadora fornecidas pelo recurso de [Conectividade](https://docs.microsoft.com/previous-versions/windows/apps/hh452990(v=win.10)). Para definir comportamentos para diferentes situações de rede, um aplicativo configura uma política de custo para cada operação usando valores definidos pela [**BackgroundTransferCostPolicy**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferCostPolicy).

Por exemplo, a política de custo definida para uma operação pode indicar que a operação deve ser pausada automaticamente quando o dispositivo estiver usando uma rede limitada. A transferência é, então, retomada (ou reiniciada) automaticamente quando uma conexão a uma rede "não restrita" é estabelecida. Para obter mais informações sobre como as redes são definidas por custo, consulte [**NetworkCostType**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkCostType).

Embora o recurso de transferência em segundo plano tenha seus próprios mecanismos para manipular alterações no status de rede, existem outras considerações gerais de conectividade para aplicativos conectados a rede. Leia sobre o [aproveitamento de informações de conexão de rede disponíveis](https://docs.microsoft.com/previous-versions/windows/apps/hh452983(v=win.10)) para saber mais.

> **Observação**  Para aplicativos executados em dispositivos móveis, existem recursos que permitem ao usuário monitorar e restringir a quantidade de dados transferida com base no tipo de conexão, no status de roaming e no plano de dados do usuário. Por isso, as transferências em segundo plano podem ser pausadas no telefone mesmo quando a [**BackgroundTransferCostPolicy**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferCostPolicy) indica que a transferência deve continuar.

A tabela a seguir indica quando as transferências em segundo plano são permitidas no telefone para cada valor da [**BackgroundTransferCostPolicy**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferCostPolicy), levando em consideração o status atual do telefone. Você pode usar a classe [**ConnectionCost**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionCost) para determinar o status atual do telefone.

| Status do dispositivo                                                                                                                      | UnrestrictedOnly | Padrão | Sempre |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------|---------|--------|
| Conectado ao WiFi                                                                                                                 | Permitir            | Permitir   | Permitir  |
| Conexão limitada, sem roaming, sob limite de dados, sob controle para permanecer dentro do limite                                                   | Negar             | Permitir   | Permitir  |
| Conexão limitada, sem roaming, sob limite de dados, sob controle para exceder o limite                                                       | Negar             | Negar    | Permitir  |
| Conexão limitada, roaming, sob limite de dados                                                                                     | Negar             | Negar    | Permitir  |
| Conexão limitada, acima do limite de dados. Esse estado ocorre somente quando o usuário habilita a opção "Restringir dados em segundo plano na interface do usuário de Data Sense". | Negar             | Negar    | Negar   |

## <a name="uploading-files"></a>Carregando arquivos
Quando a transferência em segundo plano é usada, um carregamento existe como uma [**UploadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) que expõe uma quantidade de métodos de controle usados para reiniciar ou cancelar a operação. Eventos de aplicativo (por exemplo, suspensão ou finalização) e alterações de conectividade são manipulados automaticamente pelo sistema por **UploadOperation**. Os uploads continuarão durante os períodos de suspensão ou pausa do aplicativo e prosseguirão depois da finalização do aplicativo. Além disso, a configuração da propriedade [**CostPolicy**](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.backgrounddownloader.costpolicy) indicará se seu aplicativo iniciará uploads quando uma rede limitada estiver sendo usada para conectividade com a Internet ou não.

Os exemplos a seguir orientarão você na criação e inicialização de um upload básico e mostrarão como enumerar e reintroduzir operações que persistiram desde a sessão anterior do aplicativo.

### <a name="uploading-a-single-file"></a>Carregando um único arquivo
A criação de um upload começa com [**BackgroundUploader**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundUploader). Essa classe é usada para fornecer os métodos que capacitam seu aplicativo a configurar o upload antes de criar a [**UploadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) resultante. O exemplo a seguir mostra como fazer isso com os objetos [**Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri) e [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) exigidos.

**Identificar o arquivo e o destino do upload**

Antes de podermos começar com a criação de uma [**UploadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation), primeiro precisamos identificar a URI do local para o qual carregar e o arquivo que será carregado. No exemplo a seguir, o valor *uriString* é preenchido usando uma cadeia de caracteres da entrada de interface do usuário e o valor *file* usando o objeto [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) retornado por uma operação [**PickSingleFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync).

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_B "Identify the file and destination for the upload")]

**Criar e inicializar a operação de upload**

Na etapa anterior, os valores *uriString* e *file* são passados para uma instância de nosso próximo exemplo, UploadOp, em que eles são usados para configurar e iniciar a nova operação de upload. Primeiro, *uriString* é analisada para criar o objeto [**Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri).

Em seguida, as propriedades do [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) (*file*) fornecido são usadas por [**BackgroundUploader**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundUploader) para popular o cabeçalho de solicitação e configurar a propriedade *SourceFile* com o objeto **StorageFile**. O método [**SetRequestHeader**](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.backgrounduploader.setrequestheader) é, então, chamado para inserir o nome do arquivo, fornecido como uma cadeia de caracteres, e a propriedade [**StorageFile.Name**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.name).

Por fim, [**BackgroundUploader**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundUploader) cria a [**UploadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) (*carregar*).

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_A "Create and initialize the upload operation")]

Observe as chamadas de método assíncrono definidas usando promessas do JavaScript. Observando uma linha do último exemplo:

```javascript
promise = upload.startAsync().then(complete, error, progress);
```

A chamada de método assíncrono é seguida por uma instrução then que indica métodos, definidos pelo aplicativo, que são chamados quando é retornado um resultado da chamada de método assíncrono. Para obter mais informações sobre esse padrão de programação, consulte [Programação assíncrona em JavaScript usando promessas](https://docs.microsoft.com/previous-versions/windows).

### <a name="uploading-multiple-files"></a>Carregando vários arquivos
**Identificar os arquivos e o destino do upload**

Em um cenário que envolve vários arquivos transferidos com uma só [**UploadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation), o processo começa da maneira usual, fornecendo primeiro as informações necessárias de URI de destino e de arquivo local. De modo similar ao exemplo da seção anterior, o URI é fornecido como uma cadeia de caracteres pelo usuário final e [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) pode ser usado para fornecer a capacidade de indicar arquivos também pela interface de usuário. Nesse cenário, porém, o aplicativo deve chamar o método [**PickMultipleFilesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.pickmultiplefilesasync) para permitir a seleção de vários arquivos pela interface do usuário.

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

**Criar objetos para os parâmetros fornecidos**

Os dois próximos exemplos usam o código contido em um método de exemplo único, **startMultipart**, que foi chamado no fim da última etapa. Para fins de instrução, o código no método que cria uma matriz de objetos [**BackgroundTransferContentPart**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart) foi dividido do código que cria o [**UploadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) resultante.

Primeiro, a cadeia de caracteres do URI fornecida pelo usuário é inicializada como um [**Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri). Em seguida, a matriz de objetos [**IStorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.IStorageFile) (**arquivos**) passada para esse método é iterada, cada objeto é usado para criar um novo objeto [**BackgroundTransferContentPart**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart) que, em seguida, é colocado na matriz **contentParts**.

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

**Criar e inicializar a operação de upload de várias partes**

Com nossa matriz contentParts preenchida com todos os objetos [**BackgroundTransferContentPart**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart) que representam cada [**IStorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.IStorageFile) para carregamento, estamos prontos para chamar [**CreateUploadAsync**](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.backgrounduploader.createuploadasync) usando o [**Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri) para indicar onde a solicitação será enviada.

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

### <a name="restarting-interrupted-upload-operations"></a>Reiniciando operações de upload interrompidas
Quaisquer recursos de sistema associados são liberados na conclusão ou no cancelamento de uma [**UploadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation). Contudo, se seu aplicativo for finalizado antes disso acontecer, quaisquer operações ativas serão pausadas e os recursos associados a cada uma permanecerão ocupados. Se as operações não forem enumeradas e reintroduzidas na próxima sessão de aplicativo, elas não serão concluídas e continuarão ocupando os recursos do dispositivo.

1.  Antes de definir a função que enumera operações persistidas, precisamos criar uma matriz que contenha os objetos [**UploadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) que retornarão:

    [!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_C "Restart interrupted upload operation")]

1.  Em seguida, definimos a função que enumera operações persistidas e as armazena em nossa matriz. Observe que o método **load** chamado para reatribuir revogações à [**UploadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation), caso persista com a finalização do aplicativo, está na classe UploadOp que definimos mais tarde nesta seção.

    [!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_D "Enumerate persisted operations")]

## <a name="downloading-files"></a>Baixando arquivos
Quando a transferência em segundo plano é usada, cada download existe como uma [**DownloadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation) que expõe uma quantidade de métodos de controle usados para pausar, retomar, reiniciar e cancelar a operação. Eventos de aplicativo (por exemplo, suspensão ou finalização) e alterações de conectividade são manipulados automaticamente pelo sistema por **DownloadOperation**. Os downloads continuarão durante os períodos de suspensão ou pausa do aplicativo e prosseguirão depois da finalização do aplicativo. Para situações de rede móvel, a configuração da propriedade [**CostPolicy**](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.backgrounddownloader.costpolicy) indicará se seu aplicativo iniciará ou continuará downloads quando uma rede limitada estiver sendo usada para conectividade com a Internet ou não.

Se você for baixar recursos pequenos que, provavelmente, serão completos com rapidez, é melhor usar as APIs [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) ao invés da transferência em segundo plano.

Os exemplos a seguir o guiarão através da criação e da inicialização de um download básico e como enumerar e reintroduzir operações persistidas de uma sessão de aplicativo anterior.

### <a name="configure-and-start-a-background-transfer-file-download"></a>Configurar e iniciar um download de arquivo de Transferência de Tela de Fundo
O exemplo a seguir demonstra como cadeias de caracteres representando uma URI e um nome de arquivo podem ser usadas para criar um objeto [**Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri) e o [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) que conterá o arquivo solicitado. Neste exemplo, o novo arquivo é colocado automaticamente em um local pré-definido. Como alternativa, [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) pode ser usado para permitir que os usuários indiquem onde salvar o arquivo no dispositivo. Observe que o método **load** chamado para reatribuir revogações à [**DownloadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation), caso persista com a finalização do aplicativo, está na classe DownloadOp definida mais tarde nesta seção.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_A)]

Observe as chamadas de método assíncrono definidas usando promessas do JavaScript. Olhando para a linha 17 do exemplo de código anterior:

```javascript
promise = download.startAsync().then(complete, error, progress);
```

A chamada de método assíncrono é seguida por uma instrução then que indica métodos, definidos pelo aplicativo, que são chamados quando é retornado um resultado da chamada de método assíncrono. Para obter mais informações sobre esse padrão de programação, consulte [Programação assíncrona em JavaScript usando promessas](https://docs.microsoft.com/previous-versions/windows).

### <a name="adding-additional-operation-control-methods"></a>Adicionando outros métodos de controle de operação
O nível de controle pode ser aumentado com a implementação de métodos [**DownloadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation) adicionais. Por exemplo, adicionar o seguinte código ao exemplo acima introduzirá a habilidade de cancelar o download.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_B)]

### <a name="enumerating-persisted-operations-at-start-up"></a>Enumerando operações persistidas na inicialização
Quaisquer recursos de sistema associados são liberados na conclusão ou no cancelamento de uma [**DownloadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation). Contudo, se seu aplicativo for finalizado antes da ocorrência de algum desses eventos, os downloads serão pausados e persistirão em segundo plano. Os exemplos a seguir demonstram como reintroduzir downloads persistidos em uma nova sessão de aplicativo.

1.  Antes de definir a função que enumera operações persistidas, precisamos criar uma matriz que contenha os objetos [**DownloadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation) que retornarão:

    [!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_D)]

1.  Em seguida, definimos a função que enumera operações persistidas e as armazena em nossa matriz. Observe que o método **load** chamado para reatribuir revogações para uma [**DownloadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation) persistida está no exemplo de DownloadOp que definimos mais tarde nesta seção.

    [!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_E)]

1.  Agora você pode usar a lista preenchida para reiniciar operações pendentes.

## <a name="post-processing"></a>Pós-processamento
Um novo recurso no Windows 10 é a capacidade de executar código de aplicativo na conclusão de uma transferência em segundo plano, mesmo quando o aplicativo não estiver sendo executado. Por exemplo, seu aplicativo pode atualizar uma lista de filmes disponíveis após o download de um filme, em vez de fazer seu aplicativo procurar novos filmes sempre que ele é iniciado. Ou, seu aplicativo pode manipular uma transferência de arquivo com falha por tentar novamente usar um servidor ou porta diferente. O pós-processamento é invocado para transferências com êxito e falhas, de modo que pode usá-lo para implementar manipulação de erro personalizada e lógica de repetição.

O pós-processamento usa a infraestrutura da tarefa em segundo plano existente. Crie uma tarefa em segundo plano e associe-a a suas transferências antes de iniciar as transferências. As transferências são executadas em segundo plano e, quando são concluídas, sua tarefa em segundo plano é chamada para ser executada pós-processamento.

O pós-processamento usa uma nova classe: [**BackgroundTransferCompletionGroup**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferCompletionGroup). Essa classe é similar a [**BackgroundTransferGroup**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferGroup) existente, no sentido de que permite que você agrupe as transferências em segundo plano, mas **BackgroundTransferCompletionGroup** adiciona a habilidade de designar uma tarefa em segundo plano a ser executada quando a transferência for concluída.

Você inicia uma transferência em segundo plano com pós-processamento da seguinte maneira.

1.  Crie um objeto [**BackgroundTransferCompletionGroup**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferCompletionGroup). Depois, crie um objeto [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder). Defina a propriedade **Trigger** do objeto construtor para o objeto de grupo de conclusão, e a propriedade **TaskEntryPoint** do construtor para o ponto de entrada da tarefa em segundo plano que deve ser executada na conclusão da transferência. Por fim, chame o método [**BackgroundTaskBuilder.Register**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register) para registrar sua tarefa em segundo plano. Observe que vários grupos de conclusão podem compartilhar um ponto de entrada de tarefa em segundo plano, mas você pode ter apenas um grupo de conclusão por registro de tarefa em segundo plano.

```csharp
var completionGroup = new BackgroundTransferCompletionGroup();
BackgroundTaskBuilder builder = new BackgroundTaskBuilder();

builder.Name = "MyDownloadProcessingTask";
builder.SetTrigger(completionGroup.Trigger);
builder.TaskEntryPoint = "Tasks.BackgroundDownloadProcessingTask";

BackgroundTaskRegistration downloadProcessingTask = builder.Register();
```

2.  Em seguida, você associa transferências em segundo plano ao grupo de conclusão. Depois que todas as transferências forem criadas, habilite o grupo de conclusão.

```csharp
BackgroundDownloader downloader = new BackgroundDownloader(completionGroup);
DownloadOperation download = downloader.CreateDownload(uri, file);
Task<DownloadOperation> startTask = download.StartAsync().AsTask();

// App still sees the normal completion path
startTask.ContinueWith(ForegroundCompletionHandler);

// Do not enable the CompletionGroup until after all downloads are created.
downloader.CompletinGroup.Enable();
```

3.  O código da tarefa em segundo plano extrai a lista de operações de detalhes do gatilho e seu código pode inspecionar os detalhes de cada operação e realizar o pós-processamento apropriado para cada operação.

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

A tarefa de pós-processamento é uma tarefa em segundo plano normal. Isso faz parte do pool de todas as tarefas em segundo plano, e fica sujeito à mesma política de gerenciamento de recursos de todas as tarefas em segundo plano.

Além disso, observe que o pós-processamento não substitui os manipuladores de conclusão de primeiro plano. Se seu aplicativo definir um manipulador de conclusão de primeiro plano, e seu aplicativo estiver em execução quando a transferência de arquivo for concluída, seu manipulador de conclusão de primeiro plano e seu manipulador de conclusão em segundo plano serão chamados. A ordem em que as tarefas de primeiro plano e segundo plano são chamadas não é garantida. Se você definir ambos, deverá garantir que as duas tarefas funcionem corretamente e não interfiram uma na outra se estiverem executando simultaneamente.

## <a name="request-timeouts"></a>Tempos limite de solicitação
Existem dois cenários principais de tempo limite de conexão para levar em consideração.

-   Ao estabelecer uma nova conexão para a transferência, a solicitação da conexão é anulada se não é estabelecida dentro de cinco minutos.

-   Depois de uma conexão ser estabelecida, uma mensagem de solicitação de HTTP que não tenha recebido uma resposta em dois minutos será anulada.

> **Observação**  Em qualquer um desses cenários, supondo que haja conexão com a Internet, a Transferência em Segundo Plano repetirá automaticamente a solicitação no máximo três vezes. Caso a conectividade com a Internet não seja detectada, outras solicitações esperarão até que a conexão seja estabelecida.

## <a name="debugging-guidance"></a>Instrução de depuração
Parar uma sessão de depuração no Microsoft Visual Studio é comparável a fechar seu aplicativo; uploads PUT são pausados e uploads POST são finalizados. Mesmo durante a depuração, seu aplicativo deve enumerar e então reiniciar ou cancelar quaisquer uploads que persistam. Por exemplo, você pode fazer com que seu aplicativo cancele as operações de upload enumeradas e existentes na inicialização do aplicativo se não houver interesse nas operações anteriores para essa sessão de depuração.

Enquanto numera downloads/uploads na inicialização do aplicativo, durante uma sessão de depuração, seu aplicativo poderá cancelá-los se não houver interesse nas operações anteriores para essa sessão de depuração. Observe que se houver atualizações de projeto do Visual Studio, como alterações no manifesto do aplicativo, e o aplicativo tiver sido desinstalado e reimplantado, o, [**GetCurrentUploadsAsync**](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.backgrounduploader.getcurrentuploadsasync) não poderá enumerar operações criadas usando a implantação de aplicativo anterior.

Quando usar a Transferência em segundo plano durante o desenvolvimento, pode acontecer que os caches internos das operações de transferência ativas e concluídas poderão sair da sincronização. Isso pode resultar na incapacidade de iniciar novas operações de transferência ou de interagir com as operações existentes e objetos [**BackgroundTransferGroup**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferGroup). Em alguns casos, a tentativa de interagir com operações existentes pode gerar uma falha. Isso pode ocorrer se a propriedade [**TransferBehavior**](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.backgroundtransfergroup.transferbehavior) estiver definida para **Parallel**. Esse problema ocorre somente em determinados cenários, durante o desenvolvimento, e não se aplica a usuários finais de seu aplicativo.

Quatro cenários usando o Visual Studio podem causar esse problema.

-   Você pode criar um novo projeto com o mesmo nome de aplicativo de um projeto existente, mas em uma linguagem diferente (de C++ para C#, por exemplo).
-   Você pode alterar a arquitetura de destino (de x86 para x64, por exemplo) em um projeto existente.
-   Você pode alterar a cultura (de neutra para en-US, por exemplo) em um projeto existente.
-   Você pode adicionar ou remover um recurso no manifesto do pacote (adicionando **Autenticação Empresarial**, por exemplo) em um projeto existente.

Aplicativos comuns em serviço, incluindo atualizações de manifesto, que adicionam ou removem recursos, não geram esse problema nas implantações de usuário final para seu aplicativos.
Para contornar esse problema, desinstale completamente todas as versões do aplicativo e reimplante com a nova linguagem, arquitetura, cultura ou recurso. Isso pode ser feito por meio da tela **Iniciar** ou usando PowerShell e o cmdlet **Remove-AppxPackage**.

## <a name="exceptions-in-windowsnetworkingbackgroundtransfer"></a>Exceções em Windows.Networking.BackgroundTransfer
Uma exceção é gerada quando uma cadeia de caracteres inválida do URI (Uniform Resource Identifier) é passada para o construtor do objeto [**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri).

**.NET:** O tipo [**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri) é exibido como [**System.Uri**](https://docs.microsoft.com/dotnet/api/system.uri) em C# e em VB.

No C# e no Visual Basic, esse erro pode ser evitado usando a classe [**System.Uri**](https://docs.microsoft.com/dotnet/api/system.uri) no .NET 4.5 e um dos métodos [**System.Uri.TryCreate**](https://docs.microsoft.com/dotnet/api/system.uri.trycreate#overloads) para testar a cadeia de caracteres recebida do usuário do aplicativo antes de o URI ser construído.

No C++, não há nenhum método para tentar analisar uma cadeia de caracteres para um URI. Se um aplicativo receber entrada do usuário para o [**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri), o construtor deverá estar em um bloco try/catch. Se uma exceção for lançada, o aplicativo poderá notificar o usuário e solicitar um novo nome de host.

O namespace [**Windows.Networking.backgroundTransfer**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer) tem os métodos auxiliares adequados e usa enumerações do namespace [**Windows.Networking.Sockets**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets) para lidar com os erros. Eles são úteis para resolver exceções de rede específicas de uma outra forma em seu aplicativo.

Um erro encontrado em um método assíncrono do namespace [**Windows.Networking.backgroundTransfer**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer) é retornado como um valor **HRESULT**. O método [**BackgroundTransferError.GetStatus**](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.backgroundtransfererror.getstatus) é usado para converter um erro de rede de uma operação de transferência em segundo plano em um valor de enumeração [**WebErrorStatus**](https://docs.microsoft.com/uwp/api/Windows.Web.WebErrorStatus). A maioria dos valores de enumeração **WebErrorStatus** corresponde a um erro retornado pela operação nativa de cliente HTTP ou FTP. Um aplicativo pode filtrar por um valor específico de enumeração **WebErrorStatus** para modificar o comportamento do aplicativo, dependendo da causa da exceção.

Para erros de validação de parâmetro, um aplicativo também pode usar o **HRESULT** baseado na exceção para obter informações mais detalhadas sobre o erro causador da exceção. Os valores possíveis de **HRESULT** estão listados no arquivo de cabeçalho *Winerror. h*. Para a maioria dos erros de validação de parâmetro, o **HRESULT** retornado é **E\_INVALIDARG**.

## <a name="important-apis"></a>APIs importantes
* [**Windows.Networking.BackgroundTransfer**](/uwp/api/windows.networking.backgroundtransfer)
* [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri)
* [**Windows.Networking.Sockets**](/uwp/api/Windows.Networking.Sockets)
