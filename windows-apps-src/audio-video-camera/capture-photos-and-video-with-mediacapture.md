---
ms.assetid: 1361E82A-202F-40F7-9239-56F00DFCA54B
description: Este artigo descreve as etapas para capturar fotos e vídeo usando a API do MediaCapture incluindo como inicializar e desligar o MediaCapture e como lidar as mudanças na orientação do dispositivo.
title: Capturar fotos e vídeos com o MediaCapture
---

# Capturar fotos e vídeos com o MediaCapture

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este artigo descreve as etapas para capturar fotos e vídeo usando a API do [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124), incluindo como inicializar e desligar o **MediaCapture** e como lidar com as mudanças de orientação do dispositivo.

O **MediaCapture** é fornecido para dar suporte a aplicativos que exigem controle de baixo nível do processo de captura de mídia e que implementam cenários que exigem recursos avançados de captura. Usar o **MediaCapture** também requer que você forneça sua própria interface do usuário de captura. Se seu aplicativo só precisa capturar uma foto ou vídeo, e não há preocupação com técnicas avançadas de captura, o [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) facilita a captura de uma foto ou vídeo com apenas algumas linhas de código. Para obter mais informações, consulte [Capturar fotos e vídeos com CameraCaptureUI](capture-photos-and-video-with-cameracaptureui.md).

O código neste artigo foi adaptado da [amostra CameraStarterKit](http://go.microsoft.com/fwlink/?LinkId=619479). Você pode baixar a amostra para ver o código usado no contexto ou usar a amostra como ponto de partida para seu próprio aplicativo.

## Configurar seu projeto

### Adicionar declarações de funcionalidade ao manifesto do aplicativo

Para que seu aplicativo acesse a câmera do dispositivo, você deve declarar que seu aplicativo usa as funcionalidades de *webcam* e *microphone* do dispositivo. Se você quiser salvar as fotos e os vídeos capturados na biblioteca Imagens ou Vídeos do usuário, deverá declarar também as funcionalidades *picturesLibrary* e *videosLibrary*.

**Adicionar funcionalidades ao manifesto do aplicativo**

1.  No Microsoft Visual Studio, no **Gerenciador de Soluções**, abra o designer do manifesto do aplicativo clicando duas vezes no item **package.appxmanifest**.
2.  Selecione a guia **Recursos**.
3.  Marque a caixa da **Webcam** e a caixa do **Microfone**.
4.  Para acessar as bibliotecas Imagens e Vídeos, marque as caixas para **Biblioteca de Imagens** e a caixa para **Biblioteca de Vídeos**.

### Adicionar usando diretivas para APIs relacionadas à captura de mídia

A listagem de código a seguir mostra os namespaces que são referenciados pelo código de amostra neste artigo e descreve que funcionalidade cada namespace fornece.

[!code-cs[Usando](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## Inicializar o objeto MediaCapture

A classe [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) no namespace [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) é a interface fundamental para todas as operações de captura de mídia. Os aplicativos normalmente declaram uma variável desse tipo no escopo de uma única página. Seu aplicativo precisa controlar o estado atual do **MediaCapture**, portanto, você deve declarar variáveis boolianas para o estado de inicialização, visualização e gravação do objeto.

[!code-cs[MediaCaptureVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMediaCaptureVariables)]

Para ajudar a orientar a visualização de vídeo corretamente, crie variáveis de membro para controlar se a câmera é externa e se o aplicativo está espelhando atualmente o fluxo de visualização. Seu aplicativo deve espelhar o fluxo de visualização quando você achar que o feed de vídeo está capturando o usuário porque essa é uma experiência de usuário mais natural.

[!code-cs[PreviewVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewVariables)]

O método de exemplo a seguir inicializa o objeto de captura de mídia. Primeiro, o código procura por um dispositivo de captura de vídeo que pode ser usado para captura de mídia. Uma vez encontrado, o objeto **MediaCapture** é inicializado e os manipuladores de seus eventos são registrados. Em seguida, um objeto [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/desktop/hh802710) é criado usando a ID do dispositivo de captura de vídeo. Depois, o **MediaCapture** é inicializado com uma chamada para [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598).

[!code-cs[InitializeCameraAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitializeCameraAsync)]

-   O método [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) pode ser usado para localizar todos os dispositivos de um tipo especificado. Neste exemplo, o valor de enumeração **DeviceClass.VideoCapture** é passado para indicar que somente dispositivos de captura de vídeo devem ser retornados. Observe que um dispositivo de captura de vídeo é usado para capturar fotos e vídeos.

-   **FindAllAsync** retorna um objeto [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395) que contém um objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) para cada dispositivo do tipo solicitado encontrado. O método de extensão **FirstOrDefault** do namespace **System.Linq** fornece uma sintaxe fácil para selecionar um item de uma lista com base nas condições especificadas. A primeira chamada tenta selecionar a primeira **DeviceInformation** na lista que tem um valor [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906) de **Panel.Back**, indicando que a câmera está no painel traseiro do compartimento do dispositivo. Se o dispositivo não tiver uma câmera no painel traseiro, a primeira câmera disponível será usada.

-   Se você não especificar uma ID de dispositivo quando inicializar [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573), o sistema escolherá o primeiro dispositivo em sua lista interna de dispositivos.

-   A chamada para [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) inicializa o objeto para usar o dispositivo de captura especificado. Essa chamada é feita dentro de um bloco **tentar** porque isso lançará uma **UnauthorizedAccessException** se o usuário negar o acesso do aplicativo chamador à câmera. Se a chamada tiver êxito, a variável **\_isInitialized** será configurada como true para que as chamadas de método subsequentes possam determinar se o dispositivo de captura foi inicializado.

- **Importante** Em algumas famílias de dispositivos, uma solicitação de consentimento do usuário é exibida para o usuário antes de seu aplicativo receber acesso à câmera do dispositivo. Por esse motivo, você só deve chamar [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) do thread de interface do usuário principal. Tentar iniciar a câmera de outro thread pode resultar em falha de inicialização.

-   Se a inicialização do dispositivo de captura for bem-sucedida, as variáveis serão configuradas para refletir se o dispositivo de captura é externo ou se está no painel frontal do dispositivo. Esses valores serão usados para orientar a visualização de captura corretamente para o usuário. Por fim, a interface do usuário é atualizada para refletir que a captura está disponível e o fluxo de visualização do dispositivo de captura é iniciado. Todas essas tarefas são executadas em métodos auxiliares que serão descritos neste artigo.

## Iniciar a visualização da captura

Para o usuário poder ver o que está capturando, você precisa fornecer uma visualização do que o dispositivo de captura de vídeo está vendo atualmente em sua interface do usuário.

**Importante** Você deve iniciar a visualização da captura para que o dispositivo de captura permita o foco automático, a exposição automática e a proporção de branco automática.

O controle [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) é fornecido para permitir a visualização da captura. O exemplo de código XAML a seguir define o elemento de captura.

[!code-xml[CaptureElement](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetCaptureElement)]

Os usuários esperam que a tela permaneça ligada enquanto estão visualizando a tela de captura de vídeo e não desligue devido à inatividade. Para permitir isso, você deve criar um objeto [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816). Declare essa variável com o escopo de página para que ela persista durante toda a sessão de captura.

[!code-cs[DisplayRequest](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayRequest)]

O método a seguir inicia a visualização da captura de mídia. Primeiramente, ele solicita que a tela permaneça ativa chamando [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818) na [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816). Em seguida, a visualização é iniciada chamando [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613).

[!code-cs[StartPreviewAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]

-   O método [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818) é chamado no objeto **DisplayRequest** para solicitar que o sistema deixe a tela ligada.

-   A propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) do **CaptureElement** é configurada como o objeto **MediaCapture** do aplicativo para definir a origem da visualização.

-   A propriedade [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) é fornecida pela estrutura XAML para dar suporte a interfaces do usuário bidirecionais. Definir a direção de fluxo do **CaptureElement** para [**FlowDirection.RightToLeft**](https://msdn.microsoft.com/library/windows/apps/br242397) faz com que a visualização do vídeo seja invertida horizontalmente. Isso é usado quando o dispositivo de captura está no painel frontal do dispositivo para que a visualização esteja na direção correta da perspectiva do usuário.

-   O método [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613) inicia a exibição do fluxo de visualização no **CaptureElement**. Se a visualização for iniciada com êxito, a variável **\_isPreviewing** será configurada para permitir que outras partes do aplicativo saibam que o aplicativo atualmente em visualização, e o método auxiliar para definir a rotação da visualização é chamado. Esse método é definido na próxima seção.

## Detectar a orientação da tela e do dispositivo

Existem várias áreas de um aplicativo de captura de mídia que, quando executadas em um dispositivo móvel, como um telefone ou tablet, são afetadas pela orientação atual do dispositivo. Essas áreas incluem girar corretamente o fluxo de visualização da câmera e codificar corretamente as imagens e os vídeos capturados para que, quando visualizados pelo usuário, eles estejam na orientação correta.

O termo "orientação da tela" refere-se ao modo como o sistema gira a página XAML no dispositivo para mantê-la na vertical para o usuário. "Orientação do dispositivo" refere-se à orientação do dispositivo no espaço universal e, portanto, a orientação do dispositivo de câmera físico no espaço universal. Ambos os tipos de orientação são relevantes para um aplicativo de captura de mídia. Para manipular a orientação da tela, declare e inicialize uma variável de escopo de página para a classe [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/dn264258). Declare outra variável do tipo [**DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/br226142) para controlar a orientação atual da tela. Declare uma variável [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400) e uma variável [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399) para controlar a orientação do dispositivo.

[!code-cs[DisplayInformationAndOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayInformationAndOrientation)]

Os métodos auxiliares a seguir registram e cancelam o registro de manipuladores de eventos para os eventos [**DisplayInformation.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) e [**SimpleOrientationSensor.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/br206407) e inicializam as variáveis de controle com a orientação atual. Observe que nem todos os dispositivos têm um [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400), portanto, você deve verificar antes de registrar o manipulador ou tentar obter a orientação atual.

[!code-cs[RegisterOrientationEventHandlers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterOrientationEventHandlers)]

[!code-cs[UnregisterOrientationEventHandlers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterOrientationEventHandlers)]

No manipulador de eventos para o evento **SimpleOrientationSensor.OrientationChanged**, atualize a variável de orientação do dispositivo com a orientação atual. Você não deve atualizar a orientação se o dispositivo estiver virado para cima ou para baixo.

[!code-cs[SimpleOrientationChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSimpleOrientationChanged)]

No manipulador de eventos para o evento **DisplayInformation.OrientationChanged**, atualize a variável de orientação da tela com a orientação atual. Se a visualização de vídeo do dispositivo de captura estiver sendo exibida no momento, atualize a rotação do fluxo de visualização de vídeo. O método auxiliar **SetPreviewRotationAsync** é descrito na seção a seguir.

[!code-cs[DisplayOrientationChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayOrientationChanged)]

## Definir a rotação de visualização da captura de mídia

Os usuários esperam que os controles da interface do usuário girem quando a orientação de seu dispositivo móvel muda, para que o texto da interface do usuário fique verticalmente alinhado e legível. Para o controle **CaptureElement**, porém, os usuários normalmente não querem que a orientação da visualização do vídeo gire junto com o dispositivo. Para fornecer a experiência de usuário esperada, você deve girar o fluxo de visualização para corresponder à orientação do dispositivo.

A orientação do fluxo de visualização deve ser expressa em graus. O método auxiliar a seguir converte os valores de enumeração [**DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/br226142) em graus.

[!code-cs[ConvertDisplayOrientationToDegrees](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertDisplayOrientationToDegrees)]

E este método auxiliar converte um valor de enumeração [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399), que é usado pelo [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400) para expressar a rotação do dispositivo, em graus.

[!code-cs[ConvertDeviceOrientationToDegrees](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertDeviceOrientationToDegrees)]

Em um nível inferior, a rotação de um fluxo é executada, na verdade, pela estrutura do Microsoft Media Foundation. A rotação é especificada usando o atributo [MF\_MT\_VIDEO\_ROTATION](https://msdn.microsoft.com/library/windows/desktop/hh162880). Como este é um aplicativo do Windows Runtime, a rotação é especificada usando o GUID d o atributo, em vez do nome do atributo. Defina o GUID a seguir para identificar o atributo de rotação de vídeo.

[!code-cs[RotationKey](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRotationKey)]

O método a seguir define a rotação do fluxo de visualização. O método [**GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211995) do [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) de captura de mídia retorna um conjunto de propriedades composto de pares chave/valor. [**MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640) é especificado para indicar que queremos as propriedades do fluxo de visualização de vídeo, em vez do fluxo de gravação de vídeo ou do fluxo de áudio. O conjunto de propriedades é uma interface de finalidade geral para configurar as propriedades do fluxo, mas para esta tarefa, o GUID de rotação de vídeo definido acima é adicionado como a chave de propriedade e a orientação desejada do fluxo de vídeo, em graus, é especificada como o valor. [**SetEncodingPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/dn297781) atualiza as propriedades de codificação com os novos valores. Mais uma vez, **MediaStreamType.VideoPreview** é especificado para indicar que as propriedades estão sendo configuradas para o fluxo de visualização de vídeo.

[!code-cs[SetPreviewRotation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetPreviewRotation)]

-   Para dispositivos com câmeras externas, o usuário não espera que o fluxo da câmera gire junto com o dispositivo.

-   Se a visualização estiver sendo espelhada para uma câmera no painel frontal, a direção de rotação deve ser invertida para corresponder à rotação do dispositivo.

-   Alguns dispositivos, normalmente telefones, oferecem suporte à configuração de [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/dn264259) para um valor de orientação como [**DisplayOrientations.Landscape**](https://msdn.microsoft.com/library/windows/apps/br226142) para forçar a tela a girar com o dispositivo. Você deve configurar esse valor porque ele fornece uma boa experiência em dispositivos que o aceitam, mas você ainda deve implementar o padrão acima em seu aplicativo para dar suporte a dispositivos que não aceitam preferências de rotação automática.

-   Em versões anteriores, o método [**SetPreviewRotation**](https://msdn.microsoft.com/library/windows/apps/br226611) era a única maneira de girar o fluxo de visualização. Esse método ainda está presente na superfície de API para dar suporte a aplicativos existentes, mas esse método é ineficiente e não deve ser usado para novos aplicativos.

## Capturar uma foto

O método a seguir captura uma foto usando o método [**CapturePhotoToStreamAsync**](https://msdn.microsoft.com/library/windows/apps/hh700840) e passando as propriedades de codificação solicitadas e um objeto [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241720) que conterá o resultado da operação de captura. A classe [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) fornece métodos auxiliares, como [**CreateJpeg**](https://msdn.microsoft.com/library/windows/apps/hh700994), para gerar propriedades de codificação para os tipos de arquivo compatíveis com a captura de mídia.

[!code-cs[TakePhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetTakePhotoAsync)]

Antes de salvar a foto em um arquivo, você precisa determinar a orientação correta da foto. O objeto **MediaCapture** não sabe a orientação do dispositivo e, portanto, codifica os dados da foto capturada como se o dispositivo de captura estivesse em sua orientação padrão. Isso pode resultar em uma experiência de usuário negativa quando o usuário estiver visualizando a foto capturada, já que a foto estará na orientação incorreta. Os métodos auxiliares a seguir determinam a orientação correta da foto e salvam o arquivo com a orientação correta.

O método auxiliar **GetCameraOrientation** começa com a orientação do dispositivo atual e, em seguida, gira esse valor dependendo da orientação nativa do dispositivo e do local da câmera no dispositivo. Se a câmera estiver montada no painel frontal do dispositivo, conforme indicado neste exemplo pela variável **\_mirroringPreview**, a orientação da câmera deverá ser invertida.

[!code-cs[GetCameraOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetCameraOrientation)]

O método auxiliar a seguir simplesmente converte os valores de enumeração [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399) usados pelo sensor de orientação no valor [**PhotoOrientation**](https://msdn.microsoft.com/library/windows/apps/hh965476) equivalente usado pelo codificador bitmap que será usado para salvar o arquivo.

[!code-cs[ConvertOrientationToPhotoOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertOrientationToPhotoOrientation)]

Por fim, a foto capturada pode ser codificada e salva. Crie um objeto [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) a partir do fluxo de entrada que contém os dados da foto capturada. Crie um novo [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) e abra-o para leitura e gravação. Crie um objeto [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206), passando o arquivo de saída e o decodificador que contêm os dados da imagem. Crie um novo [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/hh974338) e adicione uma nova propriedade. A chave da propriedade "System.Photo.Orientation" especifica que a propriedade representa a orientação da foto. O valor é o valor de [**PhotoOrientation**](https://msdn.microsoft.com/library/windows/apps/hh965476) que foi calculado anteriormente. Chame [**SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252) para atualizar as propriedades no codificador e, em seguida, chame [**FlushAsync**](https://msdn.microsoft.com/library/dn237883) para gravar a foto no arquivo de armazenamento.

[!code-cs[ReencodeAndSavePhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetReencodeAndSavePhotoAsync)]

-   Configurar a propriedade de bitmap "System.Photo.Orientation" codifica a orientação da foto nos metadados do arquivo. Isso não faz com que os dados reais da imagem sejam codificados de maneira diferente. Para obter mais informações sobre como inserir metadados em arquivos de imagem, consulte [Metadados de imagem](image-metadata.md).

-   Para obter mais informações sobre como trabalhar com imagens, incluindo codificação e decodificação de imagens, consulte [Geração de imagens](imaging.md).

## Capturar um vídeo

Para iniciar a captura de vídeo, crie primeiramente um arquivo de armazenamento no qual o vídeo será gravado. Em seguida, crie o [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) que o **MediaCapture** usará para codificar o vídeo para o arquivo. A classe **MediaEncodingProfile** fornece métodos, como [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/hh701078), que criam perfis de codificação para os formatos de vídeo com suporte. Use os métodos auxiliares citados anteriormente para obter a rotação correta para o vídeo, em graus. Ao contrário do cenário de foto, as informações de rotação do vídeo são codificadas no fluxo pelo **MediaCapture**. Adicione as informações de rotação ao perfil de codificação, adicionando-o à coleção [**VideoEncodingProperties.Properties**](https://msdn.microsoft.com/library/windows/apps/hh701254). O GUID definido anteriormente para rotação de vídeo é usado como a chave, e a rotação, em graus, é o valor. Por fim, chame [**MediaCapture.StartRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh700863), especificando as propriedades de codificação e o arquivo de saída para começar a gravar.

[!code-cs[StartRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartRecordingAsync)]

Para parar de gravar, chame [**MediaCapture.StopRecordAsync**](https://msdn.microsoft.com/library/windows/apps/br226623).

[!code-cs[StopRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStopRecordingAsync)]

Um manipulador para o evento [**MediaCapture.RecordLimitationExceeded**](https://msdn.microsoft.com/library/windows/apps/hh973312) foi registrado quando o **MediaCapture** foi inicializado. No manipulador, chame o método **StopRecordingAsync** para parar a gravação de vídeo.

[!code-cs[RecordLimitationExceeded](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceeded)]

## Pausar e retomar a captura de vídeo

Para alguns cenários, convém pausar e retomar uma captura de vídeo em andamento, em vez de parar a captura e iniciar uma nova. Para pausar uma gravação, chame [**PauseRecordAsync**](https://msdn.microsoft.com/library/windows/apps/dn858102). Se você especificar [**MediaCapturePauseBehavior.RetainHardwareResources**](https://msdn.microsoft.com/library/windows/apps/dn926686), o aplicativo não poderá capturar fotos enquanto o vídeo estiver em pausa nos dispositivos que não aceitam a captura simultânea de foto e vídeo. Para obter informações sobre como determinar se um dispositivo aceita a captura simultânea de foto e vídeo, consulte [Perfis de câmera](camera-profiles.md).

[!code-cs[PauseRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPauseRecordingAsync)]

Para retomar uma captura de vídeo pausada anteriormente, chame [**ResumeRecordAsync**](https://msdn.microsoft.com/library/windows/apps/dn858103).

[!code-cs[ResumeRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetResumeRecordingAsync)]

## Limpar os recursos de captura

É muito importante que você desligue e libere os recursos de captura de mídia corretamente. Não fazer isso pode causar um comportamento inesperado da câmera depois de fechar seu aplicativo, o que resulta em uma experiência de usuário negativa para seu aplicativo. As seções a seguir descrevem as diferentes etapas que você deve executar para desligar a câmera.

### Encerrar a visualização da captura

Para encerrar a visualização da captura, chame [**MediaCapture.StopPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226622) primeiro. Defina a propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) do seu [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) como null. Em seguida, chame o [**RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819) em sua variável [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) para permitir que o sistema desligue a tela quando necessário.

[!code-cs[StopPreviewAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStopPreviewAsync)]

-   Você não pode acessar a interface do usuário a partir de um thread sem interface do usuário. Portanto, é preciso configurar a propriedade **MediaElement.Source** e chamar **RequestRelease** usando o método [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) para que as chamadas sejam executadas no thread da interface do usuário.

### Encerrar e descartar o objeto MediaCapture

Antes de descartar o objeto **MediaCapture**, interrompa qualquer gravação em andamento e o fluxo de visualização chamando os métodos auxiliares definidos anteriormente. Feito isso, remova os manipuladores de eventos registrados no **MediaCapture**, chame [**Dispose**](https://msdn.microsoft.com/library/windows/apps/dn278858) para liberar os recursos associados ao objeto e configure a variável **MediaCapture** como null.

[!code-cs[CleanupCameraAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

Você deve chamar esse método para encerrar e descartar o objeto **MediaCapture** no manipulador do evento [**MediaCapture.Failed**](https://msdn.microsoft.com/library/windows/apps/br226593).

[!code-cs[CaptureFailed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureFailed)]

### Manipular eventos de ciclo de vida do aplicativo e navegação de página

Os eventos de ciclo de vida do aplicativo oferecem ao seu aplicativo a oportunidade de inicializar e liberar recursos. Isso é especialmente importante com o evento **Application.Suspending**, onde é essencial que seu aplicativo descarte corretamente os recursos de captura de mídia.

Você pode registrar manipuladores para os eventos de ciclo de vida do aplicativo no construtor da página.

[!code-cs[RegisterAppLifetimeEvents](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterAppLifetimeEvents)]

No manipulador do evento **Application.Suspending**, você deve cancelar o registro dos manipuladores dos eventos de orientação da tela e do dispositivo e encerrar o objeto **MediaCapture**. O evento [**SystemMediaTransportControls.PropertyChanged**](https://msdn.microsoft.com/library/windows/apps/dn278720) cujo registro foi cancelado aqui é necessário para outra tarefa relacionada ao ciclo de vida do aplicativo que é descrita mais adiante neste artigo.

**Cuidado** 
Você deverá solicitar um adiamento da suspensão chamando [**SuspendingOperation.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) no início do manipulador de eventos de suspensão. Isso requer que o sistema aguarde você sinalizar que a operação foi concluída antes de desativar seu aplicativo. Isso é necessário porque as operações de encerramento do **MediaCapture** são assíncronas. Portanto, o manipulador de eventos **Application.Suspending** pode ser concluído antes que a câmera seja desligada corretamente. Depois de concluir as chamadas assíncronas aguardadas, você deve liberar o adiamento chamando [**SuspendingDeferral.Complete**](https://msdn.microsoft.com/library/windows/apps/br224685).

[!code-cs[Suspending](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSuspending)]

No manipulador do evento **Application.Resuming**, você deve registrar manipuladores para os eventos de orientação da tela e do dispositivo, registrar o evento **SystemMediaTransportControls.PropertyChanged** e inicializar o objeto **MediaCapture**.

[!code-cs[Resuming](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetResuming)]

O evento [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) oferece uma oportunidade de registrar inicialmente manipuladores para os eventos de orientação da tela e do dispositivo e inicializar o objeto **MediaCapture**.

[!code-cs[OnNavigatedTo](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnNavigatedTo)]

Se seu aplicativo tiver várias páginas, você deverá limpar seus objetos de captura de mídia no manipulador de eventos [**OnNavigatingFrom**](https://msdn.microsoft.com/library/windows/apps/br227509).

[!code-cs[OnNavigatingFrom](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnNavigatingFrom)]

Para que seu aplicativo se comporte corretamente em dispositivos que aceitam várias janelas simultâneas, você deve responder quando seu aplicativo é minimizado ou restaurado. Para fazer isso, você deverá manipular o evento [**SystemMediaTransportControls.PropertyChanged**](https://msdn.microsoft.com/library/windows/apps/dn278720). Inicialize uma variável de membro para armazenar uma referência ao objeto [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) para seu aplicativo.

[!code-cs[DeclareSystemMediaTransportControls](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareSystemMediaTransportControls)]

O código para registrar e cancelar o registro do evento **PropertyChanged** deve ser adicionado aos eventos de ciclo de vida do aplicativo, conforme mostrado nos exemplos acima. No manipulador do evento, verifique se a alteração de propriedade que disparou o evento foi da propriedade [**SystemMediaTransportControlsProperty.SoundLevel**](https://msdn.microsoft.com/library/windows/apps/dn278721). Se essa foi a propriedade alterada, verifique o valor da propriedade. Se o valor for [**SoundLevel.Muted**](https://msdn.microsoft.com/library/windows/apps/hh700852), seu aplicativo foi minimizado, e você deve limpar seus recursos de captura de mídia apropriadamente. Caso contrário, o evento sinalizará que a janela do aplicativo foi restaurada, e você deverá reinicializar os recursos de captura de mídia. A propriedade **SoundLevel** deve ser acessada no thread da interface do usuário, para que o código desse método seja encapsulado em uma chamada para [**Dispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317).

[!code-cs[SystemMediaControlsPropertyChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSystemMediaControlsPropertyChanged)]

## Considerações adicionais sobre a interface do usuário para captura de mídia

### Configurar preferências de rotação automática

Como mencionado na seção anterior sobre como girar o fluxo de visualização, alguns dispositivos aceitam a configuração de [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/dn264259) para impedir que a página, inclusive o [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) que exibe a visualização, gire conforme o dispositivo. Isso proporciona uma boa experiência do usuário em dispositivos que aceitam esse recurso. Configure esse valor quando seu aplicativo for iniciado ou quando você começar a exibir a visualização. Observe que você ainda deve implementar a manipulação de rotação de visualização para dispositivos que não aceitam preferências de rotação automática.

[!code-cs[SetAutoRotationPreferences](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetAutoRotationPreferences)]

### Manipular a orientação do elemento de interface do usuário

Normalmente, os usuários esperam que os elementos de interface do usuário em um aplicativo de câmera, como os botões para iniciar a captura de foto ou vídeo, girem juntamente com a visualização de vídeo. O método a seguir ilustra o uso dos métodos auxiliares de orientação definidos anteriormente para orientar corretamente os botões na interface do usuário da câmera. Você deve chamar esse método a partir dos manipuladores de evento [**DisplayInformation.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) e [**SimpleOrientationSensor.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/br206407) e quando seu aplicativo for iniciado pela primeira vez. Sua implementação pode variar de acordo com a interface do usuário do aplicativo.

[!code-cs[UpdateButtonOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateButtonOrientation)]

Quando seu aplicativo está sendo encerrado ou se você navegar para uma página que não seja relacionada à captura de mídia, configure a preferência de rotação automática para [**None**](https://msdn.microsoft.com/library/windows/apps/br226142) para permitir que a interface do usuário gire normalmente.

[!code-cs[RevertAutoRotationPreferences](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRevertAutoRotationPreferences)]

### Suporte para captura simultânea de foto e vídeo

A API [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) permite que você capture fotos e vídeos simultaneamente em dispositivos que aceitam esse recurso. Para abreviar, este exemplo usa a propriedade [**ConcurrentRecordAndPhotoSupported**](https://msdn.microsoft.com/library/windows/apps/dn278843) para determinar se a captura simultânea de vídeo e foto é aceita, mas uma maneira mais robusta e recomendada para fazer isso é usar perfis de câmera. Para obter mais informações, consulte [Perfis de câmera](camera-profiles.md).

O método auxiliar a seguir atualiza os controles do aplicativo para corresponder o estado de captura atual do aplicativo. Chame esse método sempre que o estado de captura do seu aplicativo mudar, por exemplo, quando a captura de vídeo for iniciada ou interrompida.

[!code-cs[UpdateCaptureControls](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateCaptureControls)]

### Suporte a recursos de interface do usuário específicos para dispositivos móveis

Todo o código mostrado neste artigo funcionará em um Aplicativo Universal do Windows. Com algumas linhas adicionais de código, você pode aproveitar os recursos especiais de interface do usuário que estão presentes apenas em dispositivos móveis. Para usar esses recursos, você deve adicionar uma referência ao SDK de extensão do Microsoft Mobile para a plataforma de aplicativos universais ao seu projeto.

**Para adicionar uma referência ao SDK de extensão móvel para suporte ao botão de câmera físico**

1.  No **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência...**

2.  Expanda o nó **Universal do Windows** e selecione **Extensões**.

3.  Clique na caixa de seleção ao lado de **SDK de extensão do Microsoft Mobile para a plataforma de aplicativos universais**.

Os dispositivos móveis possuem um controle [**StatusBar**](https://msdn.microsoft.com/library/windows/apps/dn633864) que fornece ao usuário informações de status sobre o dispositivo. Esse controle ocupa espaço na tela que pode interferir com a interface do usuário de captura de mídia. Você pode ocultar a barra de status chamando [**HideAsync**](https://msdn.microsoft.com/library/windows/apps/dn610339), mas deve fazer essa chamada em um bloco condicional onde o método [**ApiInformation.IsTypePresent**](https://msdn.microsoft.com/library/windows/apps/dn949016) é usado para determinar se a API está disponível. Esse método só retornará true em dispositivos móveis que aceitam a barra de status. Você deve ocultar a barra de status quando o aplicativo for iniciado ou quando você iniciar a visualização da câmera.

[!code-cs[HideStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHideStatusBar)]

Quando seu aplicativo estiver sendo encerrado ou quando o usuário sair da página de captura de mídia do aplicativo, você tornará o controle visível novamente.

[!code-cs[ShowStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetShowStatusBar)]

Alguns dispositivos móveis possuem um botão de câmera físico dedicado que alguns usuários preferem em vez de um controle na tela. Para ser notificado quando o botão de câmera físico for pressionado, registre um manipulador para o evento [**HardwareButtons.CameraPressed**](https://msdn.microsoft.com/library/windows/apps/dn653805). Como essa API está disponível somente em dispositivos móveis, você deve usar novamente **IsTypePresent** para certificar-se de que a API é compatível com o dispositivo atual antes de tentar acessá-la.

[!code-cs[PhoneUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhoneUsing)]

[!code-cs[RegisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterCameraButtonHandler)]

No manipulador do evento **CameraPressed**, você pode iniciar uma captura de foto.

[!code-cs[CameraPressed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCameraPressed)]

Quando seu aplicativo estiver sendo encerrado ou o usuário sair da página de captura de mídia do aplicativo, cancele o registro do manipulador de botão físico.

[!code-cs[UnregisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterCameraButtonHandler)]

**Observação** Este artigo se destina a desenvolvedores do Windows 10 que escrevem aplicativos da Plataforma Universal do Windows (UWP). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

## Tópicos relacionados

* [Perfis de câmera](camera-profiles.md)
* [Captura de fotos em HDR (High Dynamic Range)](high-dynamic-range-hdr-photo-capture.md)
* [Controles de captura do dispositivo para a captura de fotos e vídeos](capture-device-controls-for-photo-and-video-capture.md)
* [Controles de dispositivo de captura para captura de vídeo](capture-device-controls-for-video-capture.md)
* [Efeitos para captura de vídeo](effects-for-video-capture.md)
* [Análise de cena para captura de mídia](scene-analysis-for-media-capture.md)
* [Sequência de fotos variável](variable-photo-sequence.md)
* [Obter um quadro de visualização](get-a-preview-frame.md)
* [Amostra do CameraStarterKit](http://go.microsoft.com/fwlink/?LinkId=619479)


<!--HONumber=Mar16_HO5-->


