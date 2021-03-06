---
Description: Este artigo descreve como criar um componente do Tempo de Execução do Windows que implemente a interface IBasicVideoEffect para permitir que você crie efeitos personalizados para fluxos de vídeo.
MS-HAID: dev\_audio\_vid\_camera.custom\_video\_effects
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Efeitos de vídeo personalizados
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 40a6bd32-a756-400f-ba34-2c5f507262c0
ms.localizationpriority: medium
ms.openlocfilehash: 1be4bf71d99bd6560ce4ed753b55dacdfcceb868
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257188"
---
# <a name="custom-video-effects"></a>Efeitos de vídeo personalizados




Este artigo descreve como criar um componente do Windows Runtime que implemente a interface [**IBasicVideoEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IBasicVideoEffect) para criar efeitos personalizados para fluxos de vídeo. Efeitos personalizados podem ser usados com várias APIs do Windows Runtime diferentes, incluindo [MediaCapture](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture), que fornece acesso à câmera do dispositivo, e [**MediaComposition**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaComposition), que permite que você crie composições complexas de clipes de mídia.

## <a name="add-a-custom-effect-to-your-app"></a>Adicionar um efeito personalizado ao seu aplicativo


Um efeito de vídeo personalizado é definido em uma classe que implementa a interface [**IBasicVideoEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IBasicVideoEffect). Essa classe não pode ser incluída diretamente no projeto do seu aplicativo. Em vez disso, você deve usar um componente do Tempo de Execução do Windows para hospedar sua classe de efeito de vídeo.

**Adicionar um componente Windows Runtime para seu efeito de vídeo**

1.  No Microsoft Visual Studio, com sua solução aberta, vá para o menu **Arquivo** e selecione **Adicionar-&gt;Novo Projeto**.
2.  Selecione o tipo de projeto **componente do Windows Runtime (Windows Universal)** .
3.  Para este exemplo, chame o projeto de *VideoEffectComponent*. Esse nome será referenciado no código posteriormente.
4.  Clique em **OK**.
5.  O modelo de projeto cria uma classe chamada Class1.cs. No **Gerenciador de Soluções**, clique com botão direito do mouse no ícone de Class1.cs e selecione **Renomear**.
6.  Renomeie o arquivo para *ExampleVideoEffect.cs*. O Visual Studio mostrará um aviso perguntando se você deseja atualizar todas as referências para o novo nome. Clique em **Sim**.
7.  Abra **ExampleVideoEffect.cs** e atualize a definição de classe para implementar a interface [**IBasicVideoEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IBasicVideoEffect).

[!code-cs[ImplementIBasicVideoEffect](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetImplementIBasicVideoEffect)]


Você precisa incluir os seguintes namespaces no arquivo de classe do efeito para acessar todos os tipos usados nos exemplos deste artigo.

[!code-cs[EffectUsing](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetEffectUsing)]


## <a name="implement-the-ibasicvideoeffect-interface-using-software-processing"></a>Implementar a interface IBasicVideoEffect usando o processamento de software


O efeito de vídeo deve implementar todos os métodos e propriedades da interface [**IBasicVideoEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IBasicVideoEffect). Esta seção percorre uma implementação simples dessa interface que usa o processamento de software.

### <a name="close-method"></a>Método Close

O sistema chamará o método [**Close**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicvideoeffect.close) em sua classe quando o efeito for encerrado. Você deve usar esse método para descartar quaisquer recursos que você criou. O argumento para o método é um [**MediaEffectClosedReason**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.MediaEffectClosedReason) que permite que você saiba se o efeito foi fechado normalmente, se ocorreu um erro ou se o efeito não é compatível com o formato de codificação necessário.

[!code-cs[Close](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetClose)]


### <a name="discardqueuedframes-method"></a>Método DiscardQueuedFrames

O método [**DiscardQueuedFrames**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicvideoeffect.discardqueuedframes) é chamado quando o efeito deve ser redefinido. Um cenário típico para isso é se o efeito armazena quadros processados anteriormente para serem usados no processamento do quadro atual. Quando esse método é chamado, você deve descartar o conjunto de quadros anteriores que foram salvos. Esse método possa ser usado para restaurar qualquer estado relacionado ao quadros anteriores, não apenas quadros de vídeo acumulados.


[!code-cs[DiscardQueuedFrames](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetDiscardQueuedFrames)]



### <a name="isreadonly-property"></a>Propriedade IsReadOnly

A propriedade [**IsReadOnly**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicvideoeffect.isreadonly) permite que o sistema saiba se o efeito será gravado na saída. Se o seu aplicativo não modifica os quadros de vídeo (por exemplo, um efeito que só executa uma análise dos quadros do vídeo), você deve definir essa propriedade como true, o que fará com que o sistema copie de forma eficiente a entrada do quadro para a saída do quadro.

> [!TIP]
> Quando a propriedade [**IsReadOnly**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicvideoeffect.isreadonly) é definida como true, o sistema copia o quadro de entrada para o quadro de saída antes que [**ProcessFrame**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicvideoeffect.processframe) seja chamada. Definir a propriedade **IsReadOnly** como true não impede que você grave nos quadros de saída do efeito em **ProcessFrame**.


[!code-cs[IsReadOnly](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetIsReadOnly)]

### <a name="setencodingproperties-method"></a>Método SetEncodingProperties

O sistema chama [**SetEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicvideoeffect.setencodingproperties) o efeito para permitir que você conheça as propriedades de codificação do fluxo de vídeo no qual o efeito está operando. Esse método também fornece uma referência para o dispositivo Direct3D usado para renderização de hardware. O uso desse dispositivo é mostrado no exemplo de processamento de hardware mais adiante neste artigo.

[!code-cs[SetEncodingProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSetEncodingProperties)]


### <a name="supportedencodingproperties-property"></a>Propriedade SupportedEncodingProperties

O sistema verifica a propriedade [**SupportedEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicvideoeffect.supportedencodingproperties) para determinar quais propriedades de codificação são compatíveis com o efeito. Observe que se o consumidor do efeito não puder codificar o vídeo usando as propriedades que você especificar, ele chamará [**Close**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicvideoeffect.close) no efeito e o removerá do pipeline de vídeo.


[!code-cs[SupportedEncodingProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSupportedEncodingProperties)]


> [!NOTE] 
> Se você retornar uma lista vazia de objetos [**VideoEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.VideoEncodingProperties) de **SupportedEncodingProperties**, o sistema usará o padrão de codificação ARGB32.

 

### <a name="supportedmemorytypes-property"></a>Propriedade SupportedMemoryTypes

O sistema verifica a propriedade [**SupportedMemoryTypes**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicvideoeffect.supportedmemorytypes) para determinar se o efeito acessará quadros de vídeo na memória do software ou na memória do hardware (GPU). Se você retornar [**MediaMemoryTypes.Cpu**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.MediaMemoryTypes), serão passados para o efeito quadros de entrada e de saída que contêm dados de imagem em objetos [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap). Se você retornar **MediaMemoryTypes.Gpu**, serão passados para o efeito quadros de entrada e de saída que contêm dados de imagem em objetos [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface).

[!code-cs[SupportedMemoryTypes](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSupportedMemoryTypes)]


> [!NOTE]
> Se você especificar [**MediaMemoryTypes.GpuAndCpu**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.MediaMemoryTypes), o sistema usará a memória do sistema ou a GPU, o que for mais eficiente para o pipeline. Ao usar esse valor, você precisa verificar o método [**ProcessFrame**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicvideoeffect.processframe) para ver se [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) ou [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) passado para o método contém dados e processar o quadro de acordo.

 

### <a name="timeindependent-property"></a>Propriedade TimeIndependent

A propriedade [**TimeIndependent**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicvideoeffect.timeindependent) permite que o sistema saiba se o efeito não requer tempo uniforme. Quando definida como true, o sistema pode usar otimizações que aprimorem o desempenho do efeito.

[!code-cs[TimeIndependent](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetTimeIndependent)]

### <a name="setproperties-method"></a>Método SetProperties

O método [**SetProperties**](https://docs.microsoft.com/uwp/api/windows.media.imediaextension.setproperties) permite que o aplicativo que está usando o efeito ajuste os parâmetros do efeito. As propriedades são passadas como um mapa de nomes e valores de propriedade [**IPropertySet**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IPropertySet).


[!code-cs[SetProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSetProperties)]


Este exemplo simples escurecerá os pixels em cada quadro de vídeo de acordo com um valor especificado. Uma propriedade é declarada e TryGetValue é usado para obter o valor definido pelo aplicativo de chamada. Se nenhum valor foi definido, um valor padrão de 0,5 será usado.

[!code-cs[FadeValue](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetFadeValue)]


### <a name="processframe-method"></a>Método ProcessFrame

O método [**ProcessFrame**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicvideoeffect.processframe) é onde o efeito modifica os dados da imagem do vídeo. O método é chamado uma vez por quadro e um objeto [**ProcessVideoFrameContext**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.ProcessVideoFrameContext) é passado. Este objeto contém um objeto [**VideoFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.VideoFrame) de entrada que contém o quadro de entrada a ser processado e um objeto **VideoFrame** de saída no qual você grava os dados da imagem que serão passados para o resto do pipeline do vídeo. Cada um desses objetos **VideoFrame** possui uma propriedade [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.media.videoframe.softwarebitmap) e uma propriedade [**Direct3DSurface**](https://docs.microsoft.com/uwp/api/windows.media.videoframe.direct3dsurface), mas o valor da propriedade [**SupportedMemoryTypes**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicvideoeffect.supportedmemorytypes) determina qual delas pode ser usada.

Este exemplo mostra uma implementação simples do método **ProcessFrame** usando processamento de software. Para obter informações sobre como trabalhar com objetos [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap), consulte [Geração de imagens](imaging.md). Uma implementação de **ProcessFrame** de exemplo usando o processamento de hardware é mostrada mais adiante neste artigo.

Acessar o buffer de dados de um **SoftwareBitmap** requer interoperabilidade COM, portanto, você deve incluir o namespace **System.Runtime.InteropServices** no arquivo de classe do efeito.

[!code-cs[COMUsing](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetCOMUsing)]


Adicione o código a seguir ao namespace para que o efeito importe a interface para acessar o buffer de imagem.

[!code-cs[COMImport](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetCOMImport)]


> [!NOTE]
> Como essa técnica acessa um buffer de imagem nativo não gerenciado, você precisará configurar seu projeto para permitir código não seguro.
> 1.  No Gerenciador de Soluções, clique com o botão direito do mouse no projeto VideoEffectComponent e selecione **Propriedades**.
> 2.  Selecione a guia **Compilar**.
> 3.  Marque a caixa de seleção **Permitir código não seguro**.

 

Agora você pode adicionar a implementação do método **ProcessFrame**. Primeiro, esse método obtém um objeto [**BitmapBuffer**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapBuffer) a partir dos bitmaps de software de entrada e de saída. Observe que o quadro de saída é aberto para gravação e o de entrada para leitura. Em seguida, um [**IMemoryBufferReference**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IMemoryBufferReference) é obtido para cada buffer, chamando-se [**CreateReference**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer.createreference). Em seguida, o buffer de dados real é obtido, transmitindo os objetos **IMemoryBufferReference** como a interface de interoperabilidade COM definida acima, **IMemoryByteAccess**, e chamando **GetBuffer**.

Agora que os buffers de dados foram obtidos, você pode ler o buffer de entrada e gravar no buffer de saída. O layout do buffer é obtido chamando [**GetPlaneDescription**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer.getplanedescription), que fornece informações sobre a largura, a distância e o deslocamento inicial do buffer. Os bits por pixel são determinados pelas propriedades de codificação definidas anteriormente com o método [**SetEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicvideoeffect.setencodingproperties). As informações de formato do buffer são usadas para encontrar o índice de cada pixel no buffer. O valor de pixel do buffer de origem é copiado para o buffer de destino, com os valores de cores sendo multiplicados pela propriedade FadeValue definida para esse efeito escurecê-las pelo valor especificado.

[!code-cs[ProcessFrameSoftwareBitmap](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetProcessFrameSoftwareBitmap)]


## <a name="implement-the-ibasicvideoeffect-interface-using-hardware-processing"></a>Implementar a interface IBasicVideoEffect usando o processamento de hardware


Criar um efeito de vídeo personalizado usando o processamento de hardware (GPU) é quase idêntico a usar o processamento de software conforme descrito anteriormente. Esta seção mostrará as algumas diferenças em um efeito que usa o processamento de hardware. Este exemplo usa a API do Windows Runtime Win2D. Para obter mais informações sobre como usar Win2D, consulte a [Documentação do Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm).

Use as etapas a seguir para adicionar o pacote NuGet Win2D ao projeto que você criou, conforme descrito na seção **Adicionar um efeito personalizado ao seu aplicativo** no início deste artigo.

**Para adicionar o pacote NuGet do Win2D ao seu projeto de efeito**

1.  No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto **VideoEffectComponent** e selecione **Gerenciar Pacotes NuGet**.
2.  Na parte superior da janela, selecione a guia **Procurar**.
3.  Na caixa de pesquisa, digite **Win2D**.
4.  Selecione **Win2D.uwp** e, em seguida, selecione **Instalar** no painel à direita.
5.  A caixa de diálogo **Revisar Alterações** mostra o pacote a ser instalado. Clique em **OK**.
6.  Aceite a licença do pacote.

Além dos namespaces incluídos na instalação do projeto básico, você precisará incluir os namespaces a seguir fornecidos pelo Win2D.

[!code-cs[UsingWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetUsingWin2D)]


Como esse efeito usará a memória GPU para operar nos dados da imagem, você deve retornar [**MediaMemoryTypes.Gpu**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.MediaMemoryTypes) da propriedade [**SupportedMemoryTypes**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicvideoeffect.supportedmemorytypes).

[!code-cs[SupportedMemoryTypesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSupportedMemoryTypesWin2D)]


Defina as propriedades de codificação com as quais o efeito será compatível com a propriedade [**SupportedEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicvideoeffect.supportedencodingproperties). Ao trabalhar com Win2D, você deve usar a codificação ARGB32.

[!code-cs[SupportedEncodingPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSupportedEncodingPropertiesWin2D)]


Use o método [**SetEncodingProperties**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) para criar um novo objeto **CanvasDevice** Win2D do [**IDirect3DDevice**](https://docs.microsoft.com/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DDevice) passado para o método.

[!code-cs[SetEncodingPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSetEncodingPropertiesWin2D)]


A implementação [**SetProperties**](https://docs.microsoft.com/uwp/api/windows.media.imediaextension.setproperties) é idêntica ao exemplo anterior de processamento de software. Este exemplo usa uma propriedade **BlurAmount** para configurar um efeito de desfoque Win2D.

[!code-cs[SetPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSetPropertiesWin2D)]

[!code-cs[BlurAmountWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetBlurAmountWin2D)]


A última etapa é implementar o método [**ProcessFrame**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicvideoeffect.processframe) que realmente processa os dados da imagem.

Usando as APIs Win2D, um **CanvasBitmap** é criado a partir da propriedade [**Direct3DSurface**](https://docs.microsoft.com/uwp/api/windows.media.videoframe.direct3dsurface) do quadro de entrada. Um **CanvasRenderTarget** é criado a partir do **Direct3DSurface** do quadro de saída e um **CanvasDrawingSession** é criado a partir desse destino de renderização. Um novo **GaussianBlurEffect** Win2D é inicializado, usando a propriedade **BlurAmount** que o nosso efeito expõe via [**SetProperties**](https://docs.microsoft.com/uwp/api/windows.media.imediaextension.setproperties). Por fim, o método **CanvasDrawingSession.DrawImage** é chamado para desenhar o bitmap de entrada para o destino de renderização usando o efeito de desfoque.

[!code-cs[ProcessFrameWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetProcessFrameWin2D)]


## <a name="adding-your-custom-effect-to-your-app"></a>Adicionando efeito personalizado ao seu aplicativo


Para usar o efeito de vídeo do seu aplicativo, você deve adicionar uma referência ao projeto do efeito ao seu aplicativo.

1.  No Gerenciador de Soluções, no projeto do seu aplicativo, clique com o botão direito do mouse em **Referências** e selecione **Adicionar referência**.
2.  Expanda a guia **Projetos**, selecione **Solução** e marque a caixa de seleção com o nome do projeto do efeito. Neste exemplo, o nome é *VideoEffectComponent*.
3.  Clique em **OK**.

### <a name="add-your-custom-effect-to-a-camera-video-stream"></a>Adicionar o efeito personalizado a um fluxo de vídeo da câmera

Você pode configurar um fluxo de visualização simples da câmera seguindo as etapas do artigo [Acesso de visualização de câmera simples](simple-camera-preview-access.md). As etapas a seguir fornecerão um objeto [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) inicializado que é usado para acessar o fluxo de vídeo da câmera.

Para adicionar o efeito de vídeo personalizado ao fluxo de uma câmera, primeiro crie um novo objeto [**VideoEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.VideoEffectDefinition), passando o namespace e o nome de classe do efeito. Em seguida, do objeto **MediaCapture**, chame o método [**AddVideoEffect**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) para adicionar o efeito ao fluxo especificado. Este exemplo usa o valor [**MediaStreamType.VideoPreview**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaStreamType) para especificar se esse efeito deve ser adicionado ao fluxo de inicialização. Se o seu aplicativo der suporte à captura de vídeo, você também poderia usar **MediaStreamType.VideoRecord** para adicionar o efeito ao fluxo de captura. **AddVideoEffect** retorna um objeto [**IMediaExtension**](https://docs.microsoft.com/uwp/api/Windows.Media.IMediaExtension) que representa o efeito personalizado. Você pode usar o método SetProperties para definir a configuração do efeito.

Depois que o efeito tiver sido adicionado, [**StartPreviewAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.startpreviewasync) é chamado para iniciar o fluxo de visualização.

[!code-cs[AddVideoEffectAsync](./code/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs#SnippetAddVideoEffectAsync)]



### <a name="add-your-custom-effect-to-a-clip-in-a-mediacomposition"></a>Adicionar o efeito personalizado a um clipe em um MediaComposition

Para obter orientações gerais sobre como criar composições de mídia a partir de clipes de vídeo, consulte [Composições e edição de mídia](media-compositions-and-editing.md). O trecho de código a seguir mostra a criação de uma composição de mídia simples com um efeito de vídeo personalizado. Um objeto [**MediaClip**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaClip) é criado, chamando-se [**CreateFromFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromfileasync), passando um arquivo de vídeo que foi selecionado pelo usuário com um [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) e o clipe é adicionado a um novo [**MediaComposition**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaComposition). Em seguida, um novo objeto [**VideoEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.VideoEffectDefinition) é criado, passando o namespace e nome de classe do efeito para o construtor. Por fim, a definição do efeito é adicionada à coleção [**VideoEffectDefinitions**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.videoeffectdefinitions) do objeto **MediaClip**.


[!code-cs[AddEffectToComposition](./code/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs#SnippetAddEffectToComposition)]


## <a name="related-topics"></a>Tópicos relacionados
* [Acesso simples à visualização da câmera](simple-camera-preview-access.md)
* [Composições de mídia e edição](media-compositions-and-editing.md)
* [Documentação do Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm)
* [Reprodução de mídia](media-playback.md)