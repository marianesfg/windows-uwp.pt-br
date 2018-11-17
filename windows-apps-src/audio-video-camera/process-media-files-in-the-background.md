---
author: drewbatgit
ms.assetid: B5E3A66D-0453-4D95-A3DB-8E650540A300
description: Este artigo mostra como usar o MediaProcessingTrigger e uma tarefa em segundo plano para processar arquivos de mídia em segundo plano.
title: Processar arquivos de mídia em segundo plano
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 866fedf35aa6f1f585825195b18cdd1fed4bad11
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7146057"
---
# <a name="process-media-files-in-the-background"></a>Processar arquivos de mídia em segundo plano



Este artigo mostra como usar o [**MediaProcessingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806005) e uma tarefa em segundo plano para processar arquivo de mídia em segundo plano.

O aplicativo de exemplo descrito neste artigo permite que o usuário selecione um arquivo de mídia de entrada para transcodificar e especificar um arquivo de saída para o resultado da transcodificação. Em seguida, uma tarefa em segundo plano é iniciada para executar a operação de transcodificação. O objetivo do [**MediaProcessingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806005) é dar suporte a diferentes situações de processamento de mídia, além de transcodificar, incluindo renderização de composições de mídia para disco e carregamento de arquivos de mídia processados depois que o processamento é concluído.

Para mais detalhes sobre os diferentes recursos do aplicativo Universal do Windows utilizados nessa amostra, consulte:

-   [Transcodificar arquivos de mídia](transcode-media-files.md)
-   [Início, retomada e tarefas em segundo plano](https://msdn.microsoft.com/library/windows/apps/mt227652)
-   [Blocos, selos e notificações](https://msdn.microsoft.com/library/windows/apps/mt185606)

## <a name="create-a-media-processing-background-task"></a>Criar uma tarefa de processamento de mídia em segundo plano

Para adicionar uma tarefa em segundo plano à sua solução existente no Microsoft Visual Studio, insira um nome para seu computador

1.  No menu **Arquivo**, selecione **Adicionar** e, em seguida, **Novo Projeto...**.
2.  Selecione o tipo de projeto **Componente do Tempo de Execução do Windows (Windows Universal)**.
3.  Insira um nome para o seu projeto de componente novo. Este exemplo usa o nome de projeto **MediaProcessingBackgroundTask**.
4.  Clique em OK.

Em **Gerenciador de Soluções**, clique com o botão direito no ícone do arquivo "Class1.cs" que é criado por padrão e selecione **Renomear**. Renomeie o arquivo para "MediaProcessingTask.cs". Quando o Visual Studio perguntar se você deseja renomear todas as referências a essa classe, clique em **Sim**.

No arquivo de classe renomeado, adicione a seguinte diretiva **using** para incluir esses namespaces no seu projeto.
                                  
[!code-cs[BackgroundUsing](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundUsing)]

Atualize sua declaração de classe para fazer a sua classe herdar de [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794).

[!code-cs[BackgroundClass](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundClass)]

Adicione as variáveis de membro a seguir à sua classe:

-   Um [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797) que será usado para atualizar o aplicativo em primeiro plano com o progresso da tarefa em segundo plano.
-   Um [**BackgroundTaskDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700499) que evita que o sistema desligue a sua tarefa em segundo plano enquanto a transcodificação de mídia está sendo executada.
-   Um objeto **CancellationTokenSource** que pode ser usado para cancelar a operação de transcodificação assíncrona.
-   O objeto [**MediaTranscoder**](https://msdn.microsoft.com/library/windows/apps/br207080) que será usado para transcodificar arquivos de mídia.

[!code-cs[BackgroundMembers](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundMembers)]

O sistema chama o método [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) de uma tarefa em segundo plano quando a tarefa é inicializada. Defina o objeto [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) passado pelo método para a variável de membro correspondente. Registre um manipulador para o evento [**Canceled**](https://msdn.microsoft.com/library/windows/apps/br224798), que será gerado se o sistema precisar desligar a tarefa em segundo plano. Então, defina a propriedade [**Progress**](https://msdn.microsoft.com/library/windows/apps/br224800) para zero.

Em seguida, chame o método do objeto de tarefa de segundo plano [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700507) para obter um adiamento. Isso informa o sistema a não desligar sua tarefa porque você está executando operações assíncronas.

Em seguida, chame o método auxiliar **TranscodeFileAsync**, que é definido na próxima seção. Se isso for concluído com êxito, um método auxiliar será chamado para inicializar uma notificação do sistema para alertar o usuário que a transcodificação foi concluída.

No fim do método **Run**, chame [**Complete**](https://msdn.microsoft.com/library/windows/apps/hh700504) no objeto de adiamento para informar ao sistema que você a sua tarefa em segundo plano foi concluída e pode ser encerrada.

[!code-cs[Run](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetRun)]

No método auxiliar **TranscodeFileAsync**, os nomes de arquivo para arquivos de saída e entrada para as operações de transcodificação são recuperados do [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) para o seu aplicativo. Esses valores serão definidos pelo seu aplicativo em primeiro plano. Crie um objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) para os arquivos de entrada e saída e, em seguida, crie um perfil de codificação para ser usado na transcodificação.

Chame [**PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936), passando no arquivo de entrada, arquivo de saída e perfil de codificação. O objeto [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941) devolvido dessa chamada informa a você se a transcodificação pode ser executada. Se a propriedade [**CanTranscode**](https://msdn.microsoft.com/library/windows/apps/hh700942) for verdadeira, chame [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) para executar a operação de transcodificação.

O método **AsTask** permite que você rastreie o progresso da operação assíncrona ou a cancele. Crie um novo objeto **Progress**, especificando as unidades de progresso que você deseja e o nome do método que será chamado para avisar você sobre o progresso atual da tarefa. Passe o objeto **Progress** no método **AsTask** juntamente com o token de cancelamento que permite que você cancele a tarefa.

[!code-cs[TranscodeFileAsync](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetTranscodeFileAsync)]

No método que você usou para criar o objeto de Progresso na etapa anterior, **Progress**, defina o progresso da instância da tarefa em segundo plano. Isso irá passar o progresso ao aplicativo em primeiro plano, se ele estiver sendo executado.

[!code-cs[Progress](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetProgress)]

O método auxiliar **SendToastNotification** cria uma nova notificação do sistema obtendo um documento XML de modelo para uma notificação que tenha apenas conteúdo de texto. O elemento de texto da notificação do sistema é definido e, em seguida, um novo objeto [**ToastNotification**](https://msdn.microsoft.com/library/windows/apps/br208641) é criado a partir do documento XML. Por fim, a notificação é exibida para o usuário chamando [**ToastNotifier.Show**](https://msdn.microsoft.com/library/windows/apps/br208659).

[!code-cs[SendToastNotification](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetSendToastNotification)]

No manipulador do evento [**Canceled**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.IBackgroundTaskInstance.Canceled), chamado quando o sistema cancela a tarefa em segundo plano, é possível registrar em log o erro para fins de telemetria.

[!code-cs[OnCanceled](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetOnCanceled)]

## <a name="register-and-launch-the-background-task"></a>Registrar e iniciar a tarefa em segundo plano

Antes de poder iniciar a tarefa em segundo plano do seu aplicativo em primeiro plano, você deve atualizar o arquivo Package.appmanifest do aplicativo em primeiro plano para informar ao sistema que seu aplicativo usa uma tarefa em segundo plano.

1.  No **Gerenciador de Soluções**, clique duas vezes no ícone do arquivo Package.appmanifest para abrir o editor de manifesto.
2.  Selecione a guia **Declarações**.
3.  Em **Declarações Disponíveis**, selecione **Tarefas em Segundo Plano** e clique em **Adicionar**.
4.  Em **Declarações Duportadas** verifique se o item **Tarefas em Segundo Plano** está selecionado. Em **Propriedades**, marque a caixa de seleção **Processamento de mídia**.
5.  Na caixa de texto **Ponto de Entrada**, especifique o namespace e nome da classe para seu teste de em segundo plano, separado por um ponto. Para este exemplo, a entrada é:
   ```csharp
   MediaProcessingBackgroundTask.MediaProcessingTask
   ```
Em seguida, você precisa adicionar uma referência à sua tarefa em segundo plano para seu aplicativo em primeiro plano.
1.  No **Gerenciador de Soluções**, no seu projeto do aplicativo em primeiro plano, clique com botão direito na pasta **Referências** e selecione **Adicionar Referência...**.
2.  Expanda o nó **Projetos** e selecione **Solução**.
3.  Marque a caixa ao lado do seu projeto de tarefa em segundo plano e clique em **OK**.

O restante do código neste exemplo deve ser adicionado ao seu aplicativo em primeiro plano. Primeiro, você precisará adicionar os namespaces a seguir ao seu projeto.

[!code-cs[ForegroundUsing](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetForegroundUsing)]

Em seguida, adicione as seguintes variáveis de membro que são necessárias para registrar a tarefa em segundo plano.

[!code-cs[ForegroundMembers](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetForegroundMembers)]

O método auxiliar **PickFilesToTranscode** usa um [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) e um [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) para abrir arquivos de entrada e saída para transcodificação. O usuário pode selecionar arquivos em uma localização a que seu aplicativo não tem acesso. Para garantir que sua tarefa em segundo plano pode abrir os arquivos, adicione-os ao [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) do seu aplicativo.

Por fim, defina entradas para nomes de arquivo de entrada e saída no [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) do seu aplicativo. A tarefa em segundo plano recupera os nomes de arquivos dessa localização.

[!code-cs[PickFilesToTranscode](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetPickFilesToTranscode)]

Para registrar a tarefa em segundo plano, crie um novo [**MediaProcessingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806005) e um novo [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Defina o nome do construtor de tarefa em segundo plano para que você possa identificá-lo mais tarde. Defina o [**TaskEntryPoint**](https://msdn.microsoft.com/library/windows/apps/br224774) para o mesmo namespace e cadeia de caracteres de nome de classe que você usou no arquivo de manifesto. Defina a propriedade [**Trigger**](https://msdn.microsoft.com/library/windows/apps/dn641725) para a instância **MediaProcessingTrigger**.

Antes de registrar a tarefa, certifique de cancelar o registro de qualquer tarefa registrada anteriormente fazendo o looping pela coleção [**AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787) e chamando [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229870) em qualquer tarefa que tenha o nome que você especificou na propriedade [**BackgroundTaskBuilder.Name**](https://msdn.microsoft.com/library/windows/apps/br224771).

Registre a tarefa em segundo plano chamando [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772). Registre manipuladores para os eventos [**Completed**](https://msdn.microsoft.com/library/windows/apps/br224788) e [**Progress**](https://msdn.microsoft.com/library/windows/apps/br224808).

[!code-cs[RegisterBackgroundTask](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetRegisterBackgroundTask)]

Um aplicativo típico registrará sua tarefa em segundo plano quando o aplicativo é iniciado inicialmente, como o evento **OnNavigatedTo** .

Inicie a tarefa em segundo plano chamando o método [**RequestAsync**](https://msdn.microsoft.com/library/windows/apps/dn765071) do objeto **MediaProcessingTrigger**. O objeto [**MediaProcessingTriggerResult**](https://msdn.microsoft.com/library/windows/apps/dn806007) devolvido por esse método informa se a tarefa em segundo plano foi iniciada com êxito ou se não foi iniciada. 

[!code-cs[LaunchBackgroundTask](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetLaunchBackgroundTask)]

Um aplicativo típico iniciará a tarefa em segundo plano em resposta à interação do usuário, como no evento de **clique** de um controle de interface do usuário.

O manipulador de evento **OnProgress** é chamado quando a tarefa em segundo plano atualiza o progresso da operação. Você pode aproveitar a oportunidade para atualizar sua interface do usuário com informações de progresso.

[!code-cs[OnProgress](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetOnProgress)]

O manipulador de evento **OnCompleted** é chamado quando a tarefa em segundo plano conclui a execução. Esta é outra oportunidade para atualizar sua interface do usuário para fornecer informações de status ao usuário.

[!code-cs[OnCompleted](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetOnCompleted)]


 

 




