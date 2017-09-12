---
author: drewbatgit
ms.assetid: a128edc8-8a80-4645-ac29-908ede2d1c72
description: "Este artigo mostra como usar um MediaFrameReader com o MediaCapture para obter quadros de mídia de uma ou mais origens disponíveis, incluindo câmeras em cores, de profundidade e infravermelho, dispositivos de áudio ou até mesmo origens personalizadas de quadros, como as que produzem quadros de rastreamento de esqueleto."
title: "Processar quadros de mídia com o MediaFrameReader"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: bc0cfc468613429d7989c9c0d93bd98246c0195b
ms.sourcegitcommit: 7540962003b38811e6336451bb03d46538b35671
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/26/2017
---
# <a name="process-media-frames-with-mediaframereader"></a>Processar quadros de mídia com o MediaFrameReader

Este artigo mostra como usar um [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) com [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) para obter quadros de mídia de uma ou mais origens disponíveis, incluindo câmeras em cores, de profundidade e infravermelho, dispositivos de áudio ou até mesmo origens personalizadas de quadros, como as que produzem quadros de rastreamento de esqueleto. Esse recurso foi criado para ser usado por aplicativos que executam processamento em tempo real de quadros de mídia, como aplicativos de câmera com reconhecimento de profundidade e realidade aumentada.

Se o seu interesse for simplesmente capturar vídeo ou fotos, como um aplicativo típico de fotografia, provavelmente você desejará usar uma das outras técnicas de captura compatíveis com o [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture). Para obter uma lista de técnicas de captura de mídia disponíveis e artigos mostrando como usá-las, consulte [**Câmera**](camera.md).

> [!NOTE] 
> Os recursos abordados neste artigo só estão disponíveis a partir do Windows 10, versão 1607.

> [!NOTE] 
> Há um exemplo de aplicativo Universal do Windows que demonstra o uso do **MediaFrameReader** para exibir quadros de origens diferentes, incluindo câmeras em cores, de profundidade e infravermelho. Para obter mais informações, consulte [Exemplo de quadros de câmera](http://go.microsoft.com/fwlink/?LinkId=823230).

## <a name="setting-up-your-project"></a>Configurando seu projeto
Como com qualquer aplicativo que usa **MediaCapture**, você deve declarar que seu aplicativo usa a funcionalidade de *webcam* antes de tentar acessar qualquer dispositivo de câmera. Se seu aplicativo fizer a captura de um dispositivo de áudio, você deve declarar também a funcionalidade do dispositivo *microfone*. 

**Adicionar funcionalidades ao manifesto do aplicativo**

1.  No Microsoft Visual Studio, no **Gerenciador de Soluções**, abra o designer do manifesto do aplicativo clicando duas vezes no item **package.appxmanifest**.
2.  Selecione a guia **Recursos**.
3.  Marque a caixa da **Webcam** e a caixa do **Microfone**.
4.  Para acessar as bibliotecas Imagens e Vídeos, marque as caixas para **Biblioteca de Imagens** e a caixa para **Biblioteca de Vídeos**.

O código de exemplo deste artigo usa APIs dos namespaces a seguir, além dos incluídos pelo modelo de projeto padrão.

[!code-cs[FramesUsing](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFramesUsing)]

## <a name="select-frame-sources-and-frame-source-groups"></a>Selecionar origens de quadro e grupos de origens de quadro
Muitos aplicativos que processam quadros de mídia precisam obter quadros de várias origens ao mesmo tempo, como câmeras de profundidade e em cores de um dispositivo. O objeto [**MediaFrameSourceGroup**] (https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup) representa um conjunto de origens de quadros de mídia que podem ser usados simultaneamente. Chame o método estático [**MediaFrameSourceGroup.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.FindAllAsync) para obter uma lista de todos os grupos de origens de quadro compatíveis com o dispositivo atual.

[!code-cs[FindAllAsync](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFindAllAsync)]

Você também pode criar um [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceWatcher) usando [**DeviceInformation.CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/br225427) e o valor retornado do [**MediaFrameSourceGroup.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.GetDeviceSelector) para receber notificações quando os grupos de origens de quadro disponíveis no dispositivo mudam, por exemplo, quando uma câmera externa é conectada. Para obter mais informações, consulte [**Enumerar dispositivos**](https://msdn.microsoft.com/windows/uwp/devices-sensors/enumerate-devices).

Um [**MediaFrameSourceGroup**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup) tem uma coleção de objetos [**MediaFrameSourceInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo) que descrevem as origens de quadro incluídas no grupo. Depois de recuperar os grupos de origens de quadro disponíveis no dispositivo, você pode selecionar o grupo que expõe as origens nas quais você está interessado.

O exemplo a seguir mostra a maneira mais simples de selecionar um grupo de origens de quadro. Esse código simplesmente faz um loop em todos os grupos disponíveis e, em seguida, faz um loop em cada item na coleção [**SourceInfos**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.SourceInfos). Cada **MediaFrameSourceInfo** é verificado para ver se dá suporte aos recursos que estamos procurando. Nesse caso, a propriedade [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo.MediaStreamType) será verificada quanto ao valor [**VideoPreview**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaStreamType), que significa que o dispositivo fornece um fluxo de visualização de vídeo, e a propriedade [**SourceKind**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo.SourceKind) será verificada quanto ao valor [**Color**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceKind), que indica se a origem fornece quadros de cor.

[!code-cs[SimpleSelect](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSimpleSelect)]

Esse método de identificação do grupo de origens de quadro e das origens desejadas funciona para casos simples, mas se você quiser selecionar origens de quadro com base em critérios mais complexos, ele pode rapidamente se tornar complicado. Outro método é usar a sintaxe Linq e objetos anônimos para fazer a seleção. O exemplo a seguir usa o método de extensão **Select** para transformar os objetos **MediaFrameSourceGroup** na lista *frameSourceGroups* em um objeto anônimo com dois campos: *sourceGroup*, que representa o grupo em si, e *colorSourceInfo*, que representa a origem de quadros de cor no grupo. O campo *colorSourceInfo* é definido como o resultado de **FirstOrDefault**, que seleciona o primeiro objeto para o qual o predicado fornecido é resolvido como true. Nesse caso, o predicado será true se o tipo de fluxo for **VideoPreview**, o tipo de origem for **Color** e a câmera estiver no painel frontal do dispositivo.

Na lista de objetos anônimos retornados na consulta descrita acima, o método de extensão **Where** é usado para selecionar apenas os objetos nos quais o campo *colorSourceInfo* não é nulo. Por fim, **FirstOrDefault** é chamado para selecionar o primeiro item da lista.

Agora você pode usar os campos do objeto selecionado para obter referências para o **MediaFrameSourceGroup** e o objeto **MediaFrameSourceInfo** que representa a câmera em cores. Eles serão usados mais tarde quando você inicializar o objeto **MediaCapture** e criar um **MediaFrameReader** para a origem selecionada. Por fim, você deve testar para ver se o grupo de origem é nulo, o que significa que o dispositivo atual não tem suas origens de captura solicitadas.

[!code-cs[SelectColor](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSelectColor)]

O exemplo a seguir usa uma técnica semelhante conforme descrito acima para selecionar um grupo de origem que contém câmeras em cores, de profundidade e infravermelho.

[!code-cs[ColorInfraredDepth](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetColorInfraredDepth)]

## <a name="initialize-the-mediacapture-object-to-use-the-selected-frame-source-group"></a>Inicializar o objeto MediaCapture para usar o grupo de origens de quadro selecionado
A próxima etapa é inicializar o objeto **MediaCapture** para usar o grupo de origens de quadro que você selecionou na etapa anterior.

O objeto **MediaCapture** normalmente é usado em vários locais em seu aplicativo, portanto, você deve declarar uma variável de membro de classe para contê-lo.

[!code-cs[DeclareMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

Crie uma instância do objeto **MediaCapture** chamando o construtor. Em seguida, crie um objeto [**MediaCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSettings) que será usado para inicializar o objeto **MediaCapture**. Neste exemplo, as configurações a seguir serão usadas:

* [**SourceGroup**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.SourceGroup) - Informa ao sistema qual grupo de origens você usará para obter os quadros. Lembre-se de que o grupo de origens define um conjunto de origens de quadros de mídia que podem ser usadas simultaneamente.
* [**SharingMode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.SharingMode) - Informa ao sistema se você precisa de controle exclusivo sobre os dispositivos de origem de captura. Se você defini-lo como [**ExclusiveControl**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSharingMode), poderá alterar as configurações do dispositivo de captura, como o formato dos quadros que ele produz, mas isso significa que se outro aplicativo já tiver controle exclusivo, ocorrerá uma falha quando ele tentar inicializar o dispositivo de captura de mídia. Se você defini-lo como [**SharedReadOnly**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSharingMode), poderá receber quadros das origens, mesmo se eles estiverem sendo usados por outro aplicativo, mas você não poderá alterar as configurações dos dispositivos.
* [**MemoryPreference**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.MemoryPreference) - Se você especificar [**CPU**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureMemoryPreference), o sistema usará a memória da CPU que garante que quando os quadros forem recebidos, eles estarão disponíveis como objetos [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap). Se você especificar [**Auto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureMemoryPreference), o sistema escolherá dinamicamente o local de memória ideal para armazenar os quadros. Se o sistema opta por usar a memória da GPU, os quadros de mídia chegarão como um objeto [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) e não como um **SoftwareBitmap**.
* [**StreamingCaptureMode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.StreamingCaptureMode) - Defina-o como [**Video**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.StreamingCaptureMode) para indicar que não é necessário o streaming de áudio.

Chame [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) para inicializar o **MediaCapture** com as configurações desejadas. Certifique-se de chamá-lo em um bloco *try* em caso de falha de inicialização.

[!code-cs[InitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitMediaCapture)]

## <a name="set-the-preferred-format-for-the-frame-source"></a>Definir o formato preferencial para a origem de quadros
Para definir o formato preferencial para uma origem de quadros, você precisa obter um objeto [**MediaFrameSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource) que represente a origem. Para obter esse objeto, acesse o dicionário [**Frames**](https://msdn.microsoft.com/library/windows/apps/Windows.Phone.Media.Capture.CameraCaptureSequence.Frames) do objeto **MediaCapture** inicializado especificando o identificador da origem de quadros que você deseja usar. Foi por essa razão que salvamos o objeto [**MediaFrameSourceInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo) quando selecionamos um grupo de origens de quadro.

A propriedade [**MediaFrameSource.SupportedFormats**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource.SupportedFormats) contém uma lista de objetos [**MediaFrameFormat**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameFormat) que descrevem os formatos com suporte para a origem de quadros. Use o método de extensão Linq **Where** para selecionar um formato com base nas propriedades desejadas. Neste exemplo, é selecionado um formato que tem uma largura de 1080 pixels e pode fornecer quadros no formato RGB de 32 bits. O método de extensão **FirstOrDefault** seleciona a primeira entrada na lista. Se o formato selecionado for nulo, o formato solicitado não será compatível com a origem de quadros. Se o formato for compatível, você poderá solicitar que a origem use esse formato chamando [**SetFormatAsync**](https://msdn.microsoft.com/library/windows/apps/).

[!code-cs[GetPreferredFormat](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetPreferredFormat)]

## <a name="create-a-frame-reader-for-the-frame-source"></a>Criar um leitor de quadros para a origem de quadros
Para receber os quadros para uma origem de quadros de mídia, use um [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader).

[!code-cs[DeclareMediaFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareMediaFrameReader)]

Instancie o leitor de quadros chamando [**CreateFrameReaderAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.CreateFrameReaderAsync) em seu objeto **MediaCapture** inicializado. O primeiro argumento para esse método é a origem da qual você deseja receber quadros. Você pode criar um leitor de quadros separado para cada origem que deseja usar. O segundo argumento informa ao sistema o formato de saída no qual deseja receber os quadros. Isso evita a necessidade de fazer suas próprias conversões em quadros ao chegarem. Observe que, se você especificar um formato que não seja compatível com a origem de quadros, uma exceção será lançada, então, certifique-se de que esse valor esteja na coleção [**SupportedFormats**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource.SupportedFormats).  

Depois de criar o leitor de quadros, registre um manipulador para o evento [**FrameArrived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.FrameArrived) que é acionado sempre que um novo quadro está disponível na origem.

Solicite que o sistema comece a ler quadros da origem chamando [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.StartAsync).

[!code-cs[CreateFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCreateFrameReader)]

## <a name="handle-the-frame-arrived-event"></a>Manipular o evento de chegada de quadros
O evento [**MediaFrameReader.FrameArrived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.FrameArrived) é acionado sempre que um novo quadro está disponível. Você pode optar por processar cada quadro recebido ou só usar os quadros quando precisar deles. Como o leitor de quadros aciona o evento em seu próprio thread, talvez seja necessário implementar alguma lógica de sincronização para certificar-se de que não esteja tentando acessar os mesmos dados de vários threads. Esta seção mostra como sincronizar desenhando quadros de cor para um controle de imagem em uma página XAML. Esse cenário aborda a restrição de sincronização adicional que exige que todas as atualizações de controles XAML sejam executadas no thread da interface do usuário.

A primeira etapa na exibição de quadros em XAML é criar um controle Image. 

[!code-xml[ImageElementXAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetImageElementXAML)]

Em sua página code-behind, declare uma variável de membro de classe do tipo **SoftwareBitmap** que será usada como um buffer de fundo no qual todas as imagens de entrada serão copiadas. Observe que os próprios dados de imagem não são copiados, apenas as referências do objeto. Além disso, declare um booliano para controlar se a operação de nossa interface do usuário estará ou não em execução.

[!code-cs[DeclareBackBuffer](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareBackBuffer)]

Como os quadros chegarão como objetos **SoftwareBitmap**, você precisa criar um objeto [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) que permita usar um **SoftwareBitmap** como a origem de um **controle** XAML. Você deve definir a origem da imagem em algum lugar no seu código antes de iniciar o leitor de quadros.

[!code-cs[ImageElementSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetImageElementSource)]

Agora é hora de implementar o manipulador de eventos **FrameArrived**. Quando o manipulador é chamado, o parâmetro *sender* contém uma referência ao objeto **MediaFrameReader** que gerou o evento. Chame [**TryAcquireLatestFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.TryAcquireLatestFrame) nesse objeto para tentar obter o quadro mais recente. Como o nome indica, **TryAcquireLatestFrame** pode não ter êxito no retorno de um quadro. Dessa forma, ao acessar as propriedades VideoMediaFrame e SoftwareBitmap, certifique-se de testar quanto ao valor nulo. Neste exemplo, o operador condicional nulo ? é usado para acessar o **SoftwareBitmap** e, em seguida, o objeto recuperado é verificado quanto ao valor nulo.

O controle **Image** só pode exibir imagens em formato BRGA8 com um valor pré-multiplicado ou nenhum alfa. Se o quadro de chegada não estiver nesse formato, o método estático [**Convert**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap.Covert) será usado para converter o bitmap de software no formato correto.

Em seguida, o método [**Interlocked.Exchange**](https://msdn.microsoft.com/library/bb337971) é usado para trocar a referência do bitmap de chegada pelo bitmap de buffer de fundo. Esse método troca essas referências em uma operação atômica thread-safe. Após a troca, a imagem antiga do buffer de fundo, agora na variável *softwareBitmap*, é descartada para limpar seus recursos.

Em seguida, o [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Core.CoreDispatcher) associado ao elemento **Image** é usado para criar uma tarefa que será executada no thread da interface do usuário chamando [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317). Como as tarefas assíncronas serão executadas dentro da tarefa, a expressão lambda transmitida para **RunAsync** é declarada com a palavra-chave *async*.

Na tarefa, a variável *_taskRunning* é verificada para garantir que apenas uma instância da tarefa seja executada por vez. Se a tarefa já não estiver em execução, *_taskRunning* será definido como true para impedir que a tarefa seja executada novamente. Em um loop *while*, **Interlocked.Exchange** é chamado para copiar do buffer de fundo em um **SoftwareBitmap** temporário até que a imagem de buffer de fundo seja nula. Para cada vez que o bitmap temporário for preenchido, a propriedade **Source** da **Image** será convertida em um **SoftwareBitmapSource** e, em seguida, [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856) será chamado para definir a origem da imagem.

Por fim, a variável *_taskRunning* é definida como false para que a tarefa possa ser executada novamente na próxima vez que o manipulador for chamado.

> [!NOTE] 
> Se você acessar a propriedade [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.VideoMediaFrame.SoftwareBitmap) ou [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.VideoMediaFrame.Direct3DSurface) fornecidas pela propriedade [**VideoMediaFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference.VideoMediaFrame) de uma [**MediaFrameReference**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference), o sistema criará uma forte referência a esses objetos, o que significa que eles não serão descartados quando você chamar [**Dispose**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference.Close) na **MediaFrameReference** contida. Você deve chamar explicitamente o método **Dispose** do **SoftwareBitmap** ou **Direct3DSurface** diretamente para os objetos serem descartados imediatamente. Caso contrário, o coletor de lixo acabará liberando a memória para esses objetos, mas você não saberá quando isso vai ocorrer, e se o número de bitmaps ou superfícies alocados exceder o valor máximo permitido pelo sistema, o fluxo de quadros novos será interrompido.


[!code-cs[FrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFrameArrived)]

## <a name="cleanup-resources"></a>Limpar os recursos
Ao terminar a leitura dos quadros, certifique-se de parar o leitor de quadros de mídia chamando [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.StopAsync), cancelando o registro do manipulador **FrameArrived** e descartando o objeto **MediaCapture**.

[!code-cs[Cleanup](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCleanup)]

Para obter mais informações sobre a limpeza de objetos de captura de mídia quando seu aplicativo é suspenso, consulte [**Exibir a visualização de câmera**](simple-camera-preview-access.md).

## <a name="the-framerenderer-helper-class"></a>A classe auxiliar FrameRenderer
O Universal Windows [Quadros de câmera de exemplo](http://go.microsoft.com/fwlink/?LinkId=823230) fornece uma classe auxiliar que torna mais fácil exibir os quadros de origens em cor, infravermelho e de profundidade em seu aplicativo. Normalmente, você vai querer fazer algo mais com dados de infravermelho e profundidade do que apenas exibi-los na tela, mas essa classe auxiliar é uma ferramenta útil para demonstrar o recurso de leitor de quadros e para a depuração de sua própria implementação do leitor de quadros.

A classe auxiliar **FrameRenderer** implementa os métodos a seguir.

* Construtor **FrameRenderer** - O construtor inicializa a classe auxiliar para usar o elemento XAML [**Image**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Image) transmitido para exibição de quadros de mídia.
* **ProcessFrame** - Esse método exibe um quadro de mídia, representado por um [**MediaFrameReference**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference), no elemento **Image** transmitido ao construtor. Normalmente você deve chamar esse método de seu manipulador de eventos [**FrameArrived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.FrameArrived), transmitindo o quadro retornado por [**TryAcquireLatestFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.TryAcquireLatestFrame).
* **ConvertToDisplayableImage** - Esse método verifica o formato do quadro de mídia e, se necessário, o converte em um formato exibível. Para imagens coloridas, isso significa garantir que o formato de cor seja BGRA8 e que o modo alfa de bitmaps seja pré-multiplicado. Para quadros de profundidade ou infravermelhos, cada linha de verificação é processada para converter os valores de profundidade ou infravermelhos em um gradiente psuedocolor, usando a classe **PsuedoColorHelper** que também está incluída no exemplo e indicada abaixo.

> [!NOTE] 
> Para fazer a manipulação de pixels em imagens **SoftwareBitmap**, você deve acessar um buffer de memória nativa. Para fazer isso, use a interface COM IMemoryBufferByteAccess incluída na listagem de código abaixo e atualize as propriedades do projeto para permitir a compilação de código não seguro. Para obter mais informações, consulte [Criar, editar e salvar imagens de bitmap](imaging.md).

[!code-cs[FrameArrived](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetFrameRenderer)]

## <a name="use-multisourcemediaframereader-to-get-time-corellated-frames-from-multiple-sources"></a>Usar MultiSourceMediaFrameReader para obter quadros correlacionados no tempo de várias fontes
A partir do Windows 10, versão 1607, é possível usar [**MultiSourceMediaFrameReader**](https://docs.microsoft.com/en-us/uwp/api/windows.media.capture.frames.multisourcemediaframereader) para receber quadros correlacionados no tempo de várias fontes. Essa API facilita a realização do processamento que requer quadros de vários fontes que foram tomados em estreita proximidade temporal, como o uso da classe [**DepthCorrelatedCoordinateMapper**](https://docs.microsoft.com/en-us/uwp/api/windows.media.devices.core.depthcorrelatedcoordinatemapper). Uma limitação do uso desse novo método é que os eventos de chegada de quadros só são gerados na taxa de origem de captura mais lenta. Quadros adicionais de fontes mais rápidas serão eliminados. Além disso, como o sistema espera que os quadros venham de diferentes fontes em taxas diferentes, ele não reconhece automaticamente se uma fonte parou de gerar quadros completamente. O exemplo de código dessa seção mostra como usar um evento para criar sua própria lógica de tempo limite que é invocada se quadros correlacionados não chegarem dentro de um limite de tempo definido pelo aplicativo.

As etapas para usar [**MultiSourceMediaFrameReader**](https://docs.microsoft.com/en-us/uwp/api/windows.media.capture.frames.multisourcemediaframereader) são semelhantes às etapas para usar [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) descritas anteriormente neste artigo. Este exemplo usará uma fonte de cor e uma fonte de profundidade. Declare algumas variáveis de cadeia de caracteres para armazenar IDs de fonte de quadro de mídia que serão usadas para selecionar quadros de cada fonte. Em seguida, declare um [**ManualResetEventSlim**](https://docs.microsoft.com/dotnet/api/system.threading.manualreseteventslim?view=netframework-4.7), um [**CancellationTokenSource**](https://msdn.microsoft.com/library/system.threading.cancellationtokensource.aspx) e um [**EventHandler**](https://msdn.microsoft.com/library/system.eventhandler.aspx) que serão usados para implementar a lógica de tempo limite do exemplo. 

[!code-cs[MultiFrameDeclarations](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameDeclarations)]

Usando as técnicas descritas anteriormente neste artigo, consulte [**MediaFrameSourceGroup**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup) que inclui as fontes de cor e profundidade necessárias para esse exemplo de cenário. Depois de selecionar o grupo de fontes de quadros desejado, obtenha o [**MediaFrameSourceInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo) para cada fonte de quadro.

[!code-cs[SelectColorAndDepth](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSelectColorAndDepth)]

Crie e inicialize um objeto **MediaCapture**, passando o grupo de fontes de quadro selecionado nas configurações de inicialização.

[!code-cs[MultiFrameInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameInitMediaCapture)]

Após inicializar o objeto **MediaCapture**, recupere os objetos [**MediaFrameSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource) para as câmeras de cor e profundidade. Armazene a ID de cada fonte para que você possa selecionar o quadro de chegada da fonte correspondente.

[!code-cs[GetColorAndDepthSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetColorAndDepthSource)]

Crie e inicialize o **MultiSourceMediaFrameReader** chamando [**CreateMultiSourceFrameReaderAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture#Windows_Media_Capture_MediaCapture_CreateMultiSourceFrameReaderAsync_Windows_Foundation_Collections_IIterable_Windows_Media_Capture_Frames_MediaFrameSource__) e passando uma matriz de fontes de quadro que usará o leitor. Registre um manipulador de eventos para o evento [**FrameArrived**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader#Windows_Media_Capture_Frames_MultiSourceMediaFrameReader_FrameArrived). Este exemplo cria uma instância da classe auxiliar **FrameRenderer**, descrita anteriormente neste artigo, para renderizar quadros para um controle **Image**. Inicie o leitor de quadros chamando [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader#Windows_Media_Capture_Frames_MultiSourceMediaFrameReader_StartAsync).

Registre um manipulador de eventos para o evento **CorellationFailed** declarado anteriormente no exemplo. Sinalizaremos esse evento se uma das fontes de quadro de mídia sendo utilizada parar de gerar quadros. Por fim, chame [**Task.Run**](https://msdn.microsoft.com/en-us/library/hh195051.aspx) para chamar o método auxiliar de tempo limite, **NotifyAboutCorrelationFailure**, em uma conversa separada. A implementação desse método é mostrada posteriormente neste artigo.

[!code-cs[InitMultiFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitMultiFrameReader)]

O evento **FrameArrived** é acionado sempre que um novo quadro está disponível em todas as fontes de quadro de mídia que são gerenciadas pela **MultiSourceMediaFrameReader**. Isso significa que o evento será gerado no ritmo de fonte de mídia mais lento. Se uma fonte gerar vários quadros no momento em que uma fonte mais lenta produzir um quadro, os quadros adicionais da fonte rápida serão descartados. 

Obtenha o [**MultiSourceMediaFrameReference**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereference) associado ao evento chamando [**TryAcquireLatestFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader#Windows_Media_Capture_Frames_MultiSourceMediaFrameReader_TryAcquireLatestFrame). Obtenha o **MediaFrameReference** associado a cada fonte de quadro de mídia chamando [**TryGetFrameReferenceBySourceId**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereference#Windows_Media_Capture_Frames_MultiSourceMediaFrameReference_TryGetFrameReferenceBySourceId_System_String_), passando as cadeias de caracteres de ID armazenadas quando o leitor de quadros foi inicializado.

Chame o método [**Set**](https://msdn.microsoft.com/library/system.threading.manualreseteventslim.set.aspx) do objeto **ManualResetEventSlim** para sinalizar que os quadros chegaram. Verificaremos esse evento no método **NotifyCorrelationFailure** que está sendo executado em uma conversa separada. 

Por fim, execute qualquer processamento nos quadros de mídia correlacionados no tempo. Este exemplo simplesmente exibe o quadro da fonte de profundidade.

[!code-cs[MultiFrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameArrived)]

O método auxiliar **NotifyCorrelationFailure** foi executado em uma conversa separada depois que o leitor de quadro foi iniciado. Nesse método, verifique se o evento recebido do quadro foi sinalizado. Lembre-se de que, no manipulador **FrameArrived**, definimos esse evento sempre que um conjunto de quadros correlacionados chegar. Se o evento não foi sinalizado por algum período de tempo definido pelo aplicativo - 5 segundo é um valor razoável - e a tarefa não foi cancelada usando o **CancellationToken**, é provável que uma das fontes de quadros de mídia parou de gerar quadros. Caso deseje desligar o leitor de quadro, gere o evento **CorrelationFailed** definido pelo aplicativo. No manipulador desse evento, é possível parar o leitor de quadro e limpar seus recursos associados conforme mostrado anteriormente neste artigo.

[!code-cs[NotifyCorrelationFailure](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetNotifyCorrelationFailure)]

[!code-cs[CorrelationFailure](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCorrelationFailure)]

## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Captura básica de fotos, áudio e vídeo com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Quadros da câmera de exemplo](http://go.microsoft.com/fwlink/?LinkId=823230)
 

 




