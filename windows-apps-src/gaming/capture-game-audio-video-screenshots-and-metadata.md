---
author: drewbatgit
ms.assetid: ''
description: Mostra como gravar áudio de jogo, vídeo e metadados para em um aplicativo UWP.
title: Capturar áudio de jogo, vídeo, capturas de tela e metadados
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, jogo, capturar, áudio, vídeo, metadados
ms.localizationpriority: medium
ms.openlocfilehash: 6c1641cb4c50b86d7f678bf18fa85ad0215b4b15
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6843133"
---
# <a name="capture-game-audio-video-screenshots-and-metadata"></a>Capturar áudio de jogo, vídeo, capturas de tela e metadados
Este artigo descreve como capturar vídeo no jogo, áudio e capturas de tela, e como enviar os metadados que o incorporará a mídia capturada e de difusão, permitindo que o app e outros criem experiências dinâmicas sincronizadas com eventos de jogo. 

Há duas maneiras diferentes de o jogo ser capturado em um aplicativo UWP. O usuário pode iniciar a captura usando a interface do usuário do sistema interno. A mídia capturada usando essa técnica é inserida no ecossistema de jogos da Microsoft, pode ser visualizada e compartilhada por meio de experiências internas, como o aplicativo XBox e não está disponível diretamente para seu aplicativo ou para os usuários. As primeiras seções deste artigo mostrarão como habilitar e desabilitar a captura de app implementada pelo sistema e como receber notificações quando a captura do app for iniciada ou interrompida.

A outra forma de capturar a mídia é usando as APIs do namespace **[Windows.Media.AppRecording](https://docs.microsoft.com/uwp/api/windows.media.apprecording)**. Se a captura for habilitada no dispositivo, o app poderá começar a captura do jogo e, após algum tempo, você poderá interromper a captura, momento em que a mídia será gravada em um arquivo. Se o usuário tiver habilitado a captura histórica, você também poderá registrar o jogo já ocorrido especificando uma hora de início no passado e uma duração de gravação. Essas duas técnicas produzem um arquivo de vídeo que pode ser acessado pelo app e, dependendo do local onde você optar por salvar os arquivos, pelo usuário. As seções intermediárias deste artigo conduzem você pela implementação desses cenários.

O namespace **[Windows.Media.Capture](https://docs.microsoft.com/uwp/api/windows.media.capture)** fornece APIs para criação de metadados que descreve o jogo que está sendo capturado ou transmitido. Isso pode incluir texto ou valores numéricos, com um rótulo de texto identificando cada item de dados. Os metadados podem representar um "evento" que ocorre em um momento específico, por exemplo, quando o usuário termina uma volta em um jogo de corrida, ou um "estado" que persiste em um intervalo de tempo, como o mapa de jogo atual do usuário que está jogando. Os metadados são gravados em um cache que é alocado e gerenciado para o app pelo sistema. Os metadados são incorporados em fluxos de transmissão e arquivos de vídeo capturados, incluindo a captura de sistema interno ou técnicas personalizadas de captura de app. As seções finais deste artigo mostram como gravar metadados de jogo.

> [!NOTE] 
> Como os metadados de jogo podem ser incorporados em arquivos de mídia que podem ser compartilhados na rede, fora do controle do usuário, você não deve incluir informações de identificação pessoal ou outros dados potencialmente confidenciais nos metadados.


## <a name="enable-and-disable-system-app-capture"></a>Habilitar e desabilitar a captura de app do sistema
A captura de app do sistema é iniciada pelo usuário com a interface do usuário do sistema interno. Os arquivos são inseridos pelo ecossistema de jogos do Windows e não estão disponíveis para seu app ou o usuário, a não ser por meio de experiências internas, como o aplicativo XBox. Seu app pode desabilitar e habilitar a captura de app iniciada pelo sistema, permitindo que você impeça a captura de determinado conteúdo ou jogo pelo usuário. 

Para habilitar ou desabilitar a captura de app do sistema, basta chamar o método estático **[AppCapture.SetAllowedAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.setallowedasync)** e passar **false** para desabilitar a captura ou **true** para habilitar a captura.

[!code-cpp[SetAppCaptureAllowed](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetSetAppCaptureAllowed)]


## <a name="receive-notifications-when-system-app-capture-starts-and-stops"></a>Receber notificações quando a captura de app do sistema for iniciada e interrompida
Para receber uma notificação quando a captura de app do sistema começar ou terminar, primeiro obtenha uma instância da classe **[AppCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture)** chamando o método de fábrica **[GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.GetForCurrentView)**. Em seguida, registre um manipulador para o evento **[CapturingChanged](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.CapturingChanged)**.

[!code-cpp[RegisterCapturingChanged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRegisterCapturingChanged)]

No manipulador do evento **CapturingChanged**, verifique as propriedades **[IsCapturingAudio](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.IsCapturingAudio)** e **[IsCapturingVideo](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.IsCapturingVideo)** para determinar se o áudio ou o vídeo está sendo capturado, respectivamente. Convém atualizar a interface do usuário do app para indicar o status de captura atual.

[!code-cpp[OnCapturingChanged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnCapturingChanged)]

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>Adicionar as Extensões de Área de Trabalho do Windows para a UWP ao app
As APIs de gravação de áudio e vídeo e de criação de capturas de tela diretamente no app, encontradas no namespace **[Windows.Media.AppRecording](https://docs.microsoft.com/uwp/api/windows.media.apprecording)**, não estão incluídas no contrato de API Universal. Para acessar as APIs, você deve adicionar ao app uma referência às Extensões de Área de Trabalho do Windows para a UWP executando as etapas a seguir.

1. No Visual Studio, em **Gerenciador de Soluções**, expanda o projeto UWP, clique com botão direito do mouse em **Referências** e selecione **Adicionar Referência...**. 
2. Expanda o nó **Universal do Windows** e selecione **Extensões**.
3. Na lista de extensões, marque a caixa de seleção ao lado da entrada **Extensões de Área de Trabalho do Windows para a UWP** que corresponde à compilação de destino do projeto. Para os recursos de transmissão de app, a versão deve ser 1709 ou superior.
4. Clique em **OK**.

## <a name="get-an-instance-of-apprecordingmanager"></a>Obter uma instância do AppRecordingManager
A classe **[AppRecordingManager](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager)** é a API central que você usará para gerenciar a gravação de app. Obtenha uma instância dessa classe chamando o método de fábrica **[GetDefault](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.GetDefault)**. Antes de usar qualquer uma das APIs do namespace **Windows.Media.AppRecording**, você deve verificar elas estão presentes no dispositivo atual. As APIs não estão disponíveis em dispositivos que executam uma versão do sistema operacional anterior ao Windows 10, versão 1709. Em vez de procurar uma versão específica do sistema operacional, use o método **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** method to query for the *Windows.Media.AppBroadcasting.AppRecordingContract* versão 1.0. Se esse contrato estiver presente, as APIs de gravação estarão disponíveis no dispositivo. O exemplo de código neste artigo procura as APIs uma vez e, em seguida, verifica se o **AppRecordingManager** é null antes das operações subsequentes.

[!code-cpp[GetAppRecordingManager](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetGetAppRecordingManager)]

## <a name="determine-if-your-app-can-currently-record"></a>Determinar se o app pode realizar uma gravação no momento
O app pode não capturar o áudio ou o vídeo por vários motivos, inclusive se o dispositivo atual não atender aos requisitos de hardware para gravação ou se outro app estiver realizando a transmissão. Antes de iniciar uma gravação, verifique se o app é capaz de realizar a gravação. Chame o método **[GetStatus](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** do objeto **AppRecordingManager** e verifique a propriedade **[CanRecord](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecord)** do objeto **[AppRecordingStatus](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus)** retornado. Se **CanRecord** retornar **false**, o que significa que o app não pode realizar a gravação no momento, verifique a propriedade **[Details](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.Details)** para descobrir o motivo. Dependendo do motivo, convém exibir o status para o usuário ou mostrar instruções para permitir que o app realize a gravação.



[!code-cpp[CanRecord](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCanRecord)]

## <a name="manually-start-and-stop-recording-your-app-to-a-file"></a>Iniciar e parar a gravação do app em um arquivo manualmente

Após verificar se o app é capaz de realizar uma gravação, você pode iniciar uma nova gravação, chamando o método **[StartRecordingToFileAsync](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.startrecordingtofileasync)** do objeto **AppRecordingManager**.

No exemplo a seguir, o primeiro bloco **then** é executado quando a tarefa assíncrona apresenta falha. O segundo bloco **then** tenta acessar o resultado da tarefa e, se o resultado for null, a tarefa foi concluída. Em ambos os casos, o método auxiliar **OnRecordingComplete**, mostrado abaixo, é chamado para manipular o resultado. 

[!code-cpp[StartRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartRecordToFile)]

Quando a operação de gravação for concluída, verifique a propriedade **[Succeeded](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** do objeto **[AppRecordingResult](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult)** retornado para determinar se a operação de registro foi bem-sucedida. Em caso afirmativo, verifique a propriedade **[IsFileTruncated](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** para determinar se, por motivos de armazenamento, o sistema foi forçado a truncar o arquivo capturado. Você pode verificar a propriedade **[Duration](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** para descobrir a duração real do arquivo gravado, que poderá ser menor que a duração da operação de gravação se o arquivo estiver truncado.

[!code-cpp[OnRecordingComplete](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnRecordingComplete)]

Os exemplos a seguir mostram alguns códigos básicos para iniciar e interromper a operação de gravação mostrada no exemplo anterior.

[!code-cpp[CallStartRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallStartRecordToFile)]

[!code-cpp[FinishRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetFinishRecordToFile)]

## <a name="record-a-historical-time-span-to-a-file"></a>Gravar um período histórico para um arquivo
Se o usuário tiver habilitado a gravação histórica para o app nas configurações do sistema, você poderá gravar um período de jogo que tenha ocorrido anteriormente. Um exemplo anterior neste artigo mostrou como confirmar se o app pode gravar um jogo atualmente. Há uma verificação adicional para determinar se a captura histórica está habilitada. Mais uma vez, chame **[GetStatus](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** e verifique a propriedade **[CanRecordTimeSpan](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecordTimeSpan)** do objeto **AppRecordingStatus** retornado. Esse exemplo também retorna a propriedade **[HistoricalBufferDuration](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.HistoricalBufferDuration)** do **AppRecordingStatus**, que será usado para determinar uma hora de início válida para a operação de gravação.

[!code-cpp[CanRecordTimeSpan](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCanRecordTimeSpan)]

Para capturar um período histórico, você deve especificar uma hora de início para a gravação e uma duração. A hora de início é fornecida como um struct **[DateTime](https://docs.microsoft.com/uwp/api/windows.foundation.datetime)**. A hora de início deve ser anterior à hora atual e estar no intervalo do buffer de gravação histórica. Neste exemplo, o tamanho do buffer é recuperado enquanto é feita a verificação para saber se a gravação histórica está habilitada, o que é mostrado no exemplo de código anterior. A duração da gravação histórica é fornecida como um struct **[TimeSpan](https://docs.microsoft.com/uwp/api/windows.foundation.timespan)**, que também deve ser igual ou menor do que a duração do buffer histórico. Após determinar a hora de início e a duração desejadas, chame **[RecordTimeSpanToFileAsync](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.recordtimespantofileasync)** para iniciar a operação de gravação.

Assim como realizar a gravação com parada e interrupção manuais, quando uma gravação histórica é concluída, você pode verificar a propriedade **[Succeeded](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** do objeto **[AppRecordingResult](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult)** retornado para determinar se a operação de gravação foi bem-sucedida e as propriedades **[IsFileTruncated](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** e **[Duration](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** para descobrir a duração real do arquivo gravado, que poderá ter uma duração menor do que o período solicitado se o arquivo estiver truncado.

[!code-cpp[RecordTimeSpanToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRecordTimeSpanToFile)]

O exemplo a seguir mostra alguns códigos básicos para iniciar a operação de gravação histórica mostrada no exemplo anterior.

[!code-cpp[CallRecordTimeSpanToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallRecordTimeSpanToFile)]

## <a name="save-screenshot-images-to-files"></a>Salvar imagens de captura de tela em arquivos
O app pode iniciar uma criação de captura de tela que salvará o conteúdo atual da janela do app em um único arquivo de imagem ou em vários arquivos de imagem com codificações de imagem diferentes. Para especificar as codificações de imagem que você gostaria de usar, crie uma lista de cadeias de caracteres em que cada uma represente um tipo de imagem. As propriedades do **[ImageEncodingSubtypes](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingsubtypes)** fornecem a cadeia de caracteres correta para cada tipo de imagem com suporte, como **MediaEncodingSubtypes.Png** ou **MediaEncodingSubtypes.JpegXr**.

Inicie a captura de tela chamando o método **[SaveScreenshotToFilesAsync](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.savescreenshottofilesasync)** do objeto **AppRecordingManager**. O primeiro parâmetro desse método é um **StorageFolder** em que os arquivos de imagem serão salvos. O segundo parâmetro é um prefixo de nome de arquivo ao qual o sistema acrescentará a extensão de cada tipo de imagem salvo, como ".png".

O terceiro parâmetro de **SaveScreenshotToFilesAsync** é necessário para que o sistema possa fazer a conversão de colorspace adequada se a janela atual a ser capturada estiver exibindo o conteúdo HDR. Se houver conteúdo HDR, esse parâmetro deverá ser definido como **AppRecordingSaveScreenshotOption.HdrContentVisible**. Caso contrário, use **AppRecordingSaveScreenshotOption.None**. O parâmetro final do método é a lista de formatos de imagem no qual a tela deve ser capturada.

Quando a chamada assíncrona para **SaveScreenshotToFilesAsync** é concluída, ela retorna um objeto **[AppRecordingSavedScreenshotInfo](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingsavedscreenshotinfo)** que fornece o **StorageFile** e o valor associado de **MediaEncodingSubtypes**, indicando o tipo de imagem de cada imagem salva.

[!code-cpp[SaveScreenShotToFiles](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetSaveScreenShotToFiles)]

O exemplo a seguir mostra alguns códigos básicos para iniciar a operação de captura de tela mostrada no exemplo anterior.

[!code-cpp[CallSaveScreenShotToFiles](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallSaveScreenShotToFiles)]

## <a name="add-game-metadata-for-system-and-app-initiated-capture"></a>Adicionar metadados de jogos para a captura iniciada por app e o sistema
As próximas seções deste artigo descrevem como fornecer metadados que o sistema incorporará no fluxo MP4 de jogos capturados ou transmitidos. Os metadados podem ser incorporados em mídias capturadas por meio da interface do usuário do sistema interno e em mídias capturadas pelo app com **AppRecordingManager**. Esses metadados podem ser extraídos pelo seu app e por outros apps durante a reprodução de mídia para proporcionar experiências contextuais sincronizadas com o jogo capturado ou transmitido.

### <a name="get-an-instance-of-appcapturemetadatawriter"></a>Obter uma instância de AppCaptureMetadataWriter
A classe principal para gerenciamento de metadados de captura de app é **[AppCaptureMetadataWriter](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter)**. Antes de inicializar uma instância dessa classe, use o método **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** para consultar o *Windows.Media.Capture.AppCaptureMetadataContract* versão 1.0 e verificar se a API está disponível no dispositivo atual.

[!code-cpp[GetMetadataWriter](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetGetMetadataWriter)]

### <a name="write-metadata-to-the-system-cache-for-your-app"></a>Gravar metadados no cache do sistema para o app
Cada item de metadados tem um rótulo de cadeia de caracteres, que identifica o item de metadados, um valor de dados associado, que pode ser uma cadeia de caracteres, um número inteiro ou um valor double, e um valor da enumeração **[AppCaptureMetadataPriority](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatapriority)**, que indica a prioridade relativa do item de dados. Um item de metadados pode ser considerado um "evento", que ocorre em um dado momento, ou um "estado", que mantém um valor durante um período. Os metadados são gravados em um cache de memória, que é alocado e gerenciado pelo sistema para o app. O sistema aplica um limite de tamanho ao cache de memória de metadados e, quando o limite for atingido, ele limpará dados com base na prioridade de gravação de cada item de metadados. A próxima seção deste artigo mostra como gerenciar a alocação de memória de metadados do app.

Um app típico pode optar por gravar alguns metadados no início da sessão de captura para fornecer algum contexto dos dados subsequentes. Nesse cenário, é recomendável que você use dados de "eventos" instantâneos. Este exemplo chama **[AddStringEvent](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.addstringevent)**, **[AddDoubleEvent](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.adddoubleevent)** e **[AddInt32Event](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.addint32event)** para definir os valores instantâneas de cada tipo de dados.

[!code-cpp[StartSession](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartSession)]

Um cenário comum para o uso de dados de "estado" que persistem ao longo do tempo é os rastreamento do mapa do jogo no qual o jogador está inserido. Este exemplo chama **[StartStringState](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.startstringstate)** para definir o valor de estado. 

[!code-cpp[StartMap](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartMap)]

Chame **[StopState](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.stopstate)** para gravar que um estado específico terminou.

[!code-cpp[EndMap](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetEndMap)]

Você pode substituir um estado definindo um novo valor com um rótulo de estado existente.

[!code-cpp[LevelUp](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetLevelUp)]

Você pode encerrar todos os estados atualmente abertos chamando **[StopAllStates](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.StopAllStates)**.

[!code-cpp[RaceComplete](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRaceComplete)]

### <a name="manage-metadata-cache-storage-limit"></a>Gerenciar o limite de armazenamento de cache de metadados
Os metadados gravados com **AppCaptureMetadataWriter** são armazenados em cache pelo sistema até que sejam gravados no streaming de mídia associado. O sistema define um limite de tamanho para o cache de metadados de cada app. Depois que o limite de tamanho de cache for atingido, o sistema começará a limpeza de metadados armazenados em cache. O sistema excluirá os metadados gravados com o valor de prioridade **[AppCaptureMetadataPriority.Informational](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatapriority)** antes de excluir os metadados com a prioridade **[AppCaptureMetadataPriority.Important](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatapriority)**.

A qualquer momento, você pode verificar o número de bytes disponíveis no cache de metadados do app chamando **[RemainingStorageBytesAvailable](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.RemainingStorageBytesAvailable)**. Você pode optar por definir seu próprio limite definido por app, após o qual você poderá optar por reduzir a quantidade de metadados gravados no cache. O exemplo a seguir mostra uma simples implementação desse padrão.

[!code-cpp[CheckMetadataStorage](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCheckMetadataStorage)]

[!code-cpp[ComboExecuted](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetComboExecuted)]

### <a name="receive-notifications-when-the-system-purges-metadata"></a>Receber notificações quando o sistema limpa os metadados
Você pode se registrar para receber uma notificação quando o sistema começar a limpar os metadados do app, registrando um manipulador para o evento **[MetadataPurged](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.MetadataPurged)**.

[!code-cpp[RegisterMetadataPurged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRegisterMetadataPurged)]

No manipulador do evento **MetadataPurged**, você pode liberar algum espaço no cache de metadados encerrando os estados de prioridade mais baixa, implementar a lógica definida pelo app para reduzir a quantidade de metadados gravada no cache ou não fazer nada e deixar que o sistema continue limpando o cache com base na prioridade de gravação.

[!code-cpp[OnMetadataPurged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnMetadataPurged)]

## <a name="related-topics"></a>Tópicos relacionados

* [Jogos](index.md)
 

 




