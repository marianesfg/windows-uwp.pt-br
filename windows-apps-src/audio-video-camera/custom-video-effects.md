---
author: drewbatgit
Description: "Este artigo descreve como criar um componente do Tempo de Execução do Windows que implemente a interface IBasicVideoEffect para permitir que você crie efeitos personalizados para fluxos de vídeo."
MS-HAID: dev\_audio\_vid\_camera.custom\_video\_effects
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "Efeitos de vídeo personalizados"
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: d6ad5b2488f79787c07b4057b34fcbfd3a4df3c3

---

# Efeitos de vídeo personalizados


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


\[Algumas informações dizem respeito a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não fornece nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.\]

Este artigo descreve como criar um Compontente do Windows Runtime que implemente a interface [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788) para permitir que você crie efeitos personalizados para fluxos de vídeo. Efeitos personalizados podem ser usados com várias APIs do Windows Runtime diferentes, incluindo [MediaCapture](https://msdn.microsoft.com/library/windows/apps/br241124), que fornece acesso à câmera do dispositivo, e [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646), que permite que você crie composições complexas de clipes de mídia.

## Adicionar um efeito personalizado ao seu aplicativo


Um efeito de vídeo personalizado é definido em uma classe que implementa a interface [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788). Essa classe não pode ser incluída diretamente no projeto do seu aplicativo. Em vez disso, você deve usar um componente do Tempo de Execução do Windows para hospedar sua classe de efeito de vídeo.

**Adicionar um componente do Tempo de Execução do Windows para o efeito de vídeo**

1.  No Microsoft Visual Studio, com sua solução aberta, vá para o menu **Arquivo** e selecione **Adicionar-&gt;Novo Projeto....**
2.  Selecione o tipo de projeto **Componente do Tempo de Execução do Windows (Windows Universal)**.
3.  Para este exemplo, chame o projeto de "VideoEffectComponent". Esse nome será referenciado no código posteriormente.
4.  Clique em **OK**.
5.  O modelo de projeto cria uma classe chamada Class1.cs. No **Gerenciador de Soluções**, clique com botão direito do mouse no ícone de Class1.cs e selecione **Renomear**.
6.  Renomeie o arquivo para "ExampleVideoEffect.cs". O Visual Studio mostrará um aviso perguntando se você deseja atualizar todas as referências para o novo nome. Clique em **Sim**.
7.  Abra "ExampleVideoEffect.cs" e atualize a definição de classe para implementar a interface [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788).

[!code-cs[ImplementIBasicVideoEffect](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetImplementIBasicVideoEffect)]


Você precisa incluir os seguintes namespaces no arquivo de classe do efeito para acessar todos os tipos usados nos exemplos deste artigo.

[!code-cs[EffectUsing](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetEffectUsing)]


## Implementar a interface IBasicVideoEffect usando o processamento de software


O efeito de vídeo deve implementar todos os métodos e propriedades da interface [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788). Esta seção percorre uma implementação simples dessa interface que usa o processamento de software.

### Método Close

O sistema chamará o método [**Close**](https://msdn.microsoft.com/library/windows/apps/dn764789) em sua classe quando o efeito for encerrado. Você deve usar esse método para descartar quaisquer recursos que você criou. O argumento para o método é um MediaEffectClosedReason que permite que você saiba se o efeito foi fechado normalmente, se ocorreu um erro ou se o efeito não é compatível com o formato de codificação necessário.

[!code-cs[Fechar](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetClose)]


### Método DiscardQueuedFrames

O método [**DiscardQueuedFrames**](https://msdn.microsoft.com/library/windows/apps/dn764790) é chamado quando o efeito deve ser redefinido. Um cenário típico para isso é se o efeito armazena quadros processados anteriormente para serem usados no processamento do quadro atual. Quando esse método é chamado, você deve descartar o conjunto de quadros anteriores que foram salvos. Embora esse método possa ser usado para restaurar qualquer estado relacionado ao quadros anteriores, não apenas quadros de vídeo acumulados.


[!code-cs[DiscardQueuedFrames](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetDiscardQueuedFrames)]



### Propriedade IsReadOnly

A propriedade [**IsReadOnly**](https://msdn.microsoft.com/library/windows/apps/dn764792) permite que o sistema saiba se o efeito será gravado na saída. Se o seu aplicativo não modifica os quadros de vídeo - por exemplo, um efeito que só executa uma análise dos quadros do vídeo - você deve definir essa propriedade como true, o que fará com que o sistema copie de forma eficiente a entrada do quadro para a saída do quadro.


            **Dica**  Quando a propriedade [**IsReadOnly**](https://msdn.microsoft.com/library/windows/apps/dn764792) é definida como true, o sistema copia o quadro de entrada para o quadro de saída antes que [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764794) seja chamada. Definir a propriedade **IsReadOnly** como true não impede que você grave nos quadros de saída do efeito em **ProcessFrame**.

[!code-cs[IsReadOnly](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetIsReadOnly)] 


### Método SetEncodingProperties

O sistema chama [**SetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn919884) o efeito para permitir que você conheça as propriedades de codificação do fluxo de vídeo no qual o efeito está operando. Esse método também fornece uma referência para o dispositivo Direct3D usado para renderização de hardware. O uso desse dispositivo é mostrado no exemplo de processamento de hardware mais adiante neste artigo.

[!code-cs[SetEncodingProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSetEncodingProperties)]


### Propriedade SupportedEncodingProperties

O sistema verifica a propriedade [**SupportedEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn764799) para determinar quais propriedades de codificação são compatíveis com o efeito. Observe que se o consumidor do efeito não puder codificar o vídeo usando as propriedades que você especificar, ele chamará [**Close**](https://msdn.microsoft.com/library/windows/apps/dn764789) no efeito e o removerá do pipeline de vídeo.


[!code-cs[SupportedEncodingProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSupportedEncodingProperties)]



            **Observação**  Se você retornar uma lista vazia de objetos [**VideoEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701217) de **SupportedEncodingProperties**, o sistema usará o padrão de codificação ARGB32.

 

### Propriedade SupportedMemoryTypes

O sistema verifica a propriedade [**SupportedMemoryTypes**](https://msdn.microsoft.com/library/windows/apps/dn764801) para determinar se o efeito acessará quadros de vídeo na memória do software ou na memória do hardware (GPU). Se você retornar [**MediaMemoryTypes.Cpu**](https://msdn.microsoft.com/library/windows/apps/dn764822), serão passados para o efeito quadros de entrada e de saída que contêm dados de imagem em objetos [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358). Se você retornar **MediaMemoryTypes.Gpu**, serão passados para o efeito quadros de entrada e de saída que contêm dados de imagem em objetos [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505).

[!code-cs[SupportedMemoryTypes](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSupportedMemoryTypes)]



            **Observação**  Se você especificar [**MediaMemoryTypes.GpuAndCpu**](https://msdn.microsoft.com/library/windows/apps/dn764822), o sistema usará a memória do sistema ou a GPU, o que for mais eficiente para o pipeline. Ao usar esse valor, você precisa verificar o método [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764794) para ver se [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) ou [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505) passado para o método contém dados e processar o quadro de acordo.

 

### Propriedade TimeIndependent

A propriedade [**TimeIndependent**](https://msdn.microsoft.com/library/windows/apps/dn764803) permite que o sistema saiba se o efeito requer tempo uniforme. Quando definida como true, o sistema pode usar otimizações que aprimorem o desempenho do efeito.

[!code-cs[TimeIndependent](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetTimeIndependent)]

### Método SetProperties

O método [**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986) permite que o aplicativo que está usando o efeito ajuste os parâmetros do efeito. As propriedades são passadas como um mapa de nomes e valores de propriedade [**IPropertySet**](https://msdn.microsoft.com/library/windows/apps/br226054).


[!code-cs[SetProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSetProperties)]


Este exemplo simples escurecerá os pixels em cada quadro de vídeo de acordo com um valor especificado. Uma propriedade é declarada e TryGetValue é usado para obter o valor definido pelo aplicativo de chamada. Se nenhum valor foi definido, um valor padrão de. 5 será usado.

[!code-cs[FadeValue](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetFadeValue)]


### Método ProcessFrame

O método [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764794) é onde o efeito modifica os dados da imagem do vídeo. O método é chamado uma vez por quadro e um objeto [**ProcessVideoFrameContext**](https://msdn.microsoft.com/library/windows/apps/dn764826) é passado. Este objeto contém um objeto [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917) de entrada que contém o quadro de entrada a ser processado e um objeto **VideoFrame** de saída no qual você grava os dados da imagem que serão passados para o resto do pipeline do vídeo. Cada um desses objetos **VideoFrame** possui uma propriedade [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn930926) e uma propriedade [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn930920), mas o valor da propriedade [**SupportedMemoryTypes**](https://msdn.microsoft.com/library/windows/apps/dn764801) determina qual delas pode ser usada.

Este exemplo mostra uma implementação simples do método **ProcessFrame** usando processamento de software. Para obter informações sobre como trabalhar com objetos [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358), consulte [Geração de imagens](imaging.md). Uma implementação de **ProcessFrame** de exemplo usando o processamento de hardware é mostrada mais adiante neste artigo.

Acessar o buffer de dados de um **SoftwareBitmap** requer interoperabilidade COM, portanto, você deve incluir o namespace **System.Runtime.InteropServices** no arquivo de classe do efeito.

[!code-cs[COMUsing](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetCOMUsing)]


Adicione o código a seguir ao namespace para que o efeito importe a interface para acessar o buffer de imagem.

[!code-cs[COMImport](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetCOMImport)]



            **Observação**  Como essa técnica acessa um buffer de imagem nativo não gerenciado, você precisará configurar seu projeto para permitir código não seguro.
1.  No Gerenciador de Soluções, clique com o botão direito do mouse no projeto VideoEffectComponent e selecione Propriedades...
2.  Selecione a guia Compilar.
3.  Marque a caixa de seleção de "Permitir código não seguro"

 

Agora você pode adicionar a implementação do método **ProcessFrame**. Primeiro, esse método obtém um objeto [**BitmapBuffer**](https://msdn.microsoft.com/library/windows/apps/dn887325) a partir dos bitmaps de software de entrada e de saída. Observe que o quadro de saída é aberto para gravação e o de entrada para leitura. Em seguida, um [**IMemoryBufferReference**](https://msdn.microsoft.com/library/windows/apps/dn921671) é obtido para cada buffer, chamando-se [**CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn949046). Em seguida, o buffer de dados real é obtido, transmitindo os objetos **IMemoryBufferReference** como a interface de interoperabilidade COM definida acima, **IMemoryByteAccess**, e chamando **GetBuffer**.

Agora que os buffers de dados foram obtidos, você pode ler o buffer de entrada e gravar no buffer de saída. O layout do buffer é obtido chamando [**GetPlaneDescription**](https://msdn.microsoft.com/library/windows/apps/dn887330), que fornece informações sobre a largura, a distância e o deslocamento inicial do buffer. Os bits por pixel são determinados pelas propriedades de codificação definidas anteriormente com o método [**SetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn919884). As informações de formato do buffer são usadas para encontrar o índice de cada pixel no buffer. O valor de pixel do buffer de origem é copiado para o buffer de destino, com os valores de cores sendo multiplicados pela propriedade FadeValue definida para esse efeito escurecê-las pelo valor especificado.

[!code-cs[ProcessFrameSoftwareBitmap](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetProcessFrameSoftwareBitmap)]


## Implementar a interface IBasicVideoEffect usando o processamento de hardware


Criar um efeito de vídeo personalizado usando o processamento de hardware (GPU) é quase idêntico a usar o processamento de software conforme descrito anteriormente. Esta seção mostrará as algumas diferenças em um efeito que usa o processamento de hardware. Este exemplo usa a API do Windows Runtime Win2D. Para obter mais informações sobre como usar Win2D, consulte a [Documentação do Win2D](http://go.microsoft.com/fwlink/?LinkId=519078).

Use as etapas a seguir para adicionar o pacote NuGet Win2D ao projeto que você criou, conforme descrito na seção [Adicionar um efeito personalizado ao seu aplicativo](#addacustomeffect) no início deste artigo.

**Adicionar o pacote NuGet Win2D ao seu projeto de efeito**

1.  No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto **VideoEffectComponent** e selecione **Gerenciar Pacotes NuGet...**
2.  Na parte superior da janela, selecione a guia **Procurar**.
3.  Na caixa de pesquisa, digite "Win2D".
4.  Clique em **Win2D.uwp** e em Instalar no painel à direita.
5.  A caixa de diálogo **Revisar Alterações** mostra o pacote a ser instalado. Clique em **OK**.
6.  Aceite a licença do pacote.

Além dos namespaces incluídos na instalação do projeto básico, você precisará incluir os namespaces a seguir fornecidos pelo Win2D.

[!code-cs[UsingWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetUsingWin2D)]


Como esse efeito usará a memória GPU para operar nos dados da imagem, você deve retornar [**MediaMemoryTypes.Gpu**](https://msdn.microsoft.com/library/windows/apps/dn764822) da propriedade [**SupportedMemoryTypes**](https://msdn.microsoft.com/library/windows/apps/dn764801).

[!code-cs[SupportedMemoryTypesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSupportedMemoryTypesWin2D)]


Defina as propriedades de codificação com as quais o efeito será compatível com a propriedade [**SupportedEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn764799). Ao trabalhar com Win2D, você deve usar a codificação ARGB32.

[!code-cs[SupportedEncodingPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSupportedEncodingPropertiesWin2D)]


Use o método [**SetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn919884) para criar um novo objeto **CanvasDevice** Win2D do [**IDirect3DDevice**](https://msdn.microsoft.com/library/windows/apps/dn895092) passado para o método.

[!code-cs[SetEncodingPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSetEncodingPropertiesWin2D)]


A implementação [**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986) é idêntica ao exemplo de processamento de software acima. Este exemplo usa uma propriedade **BlurAmount** para configurar um efeito de desfoque Win2D.

[!code-cs[SetPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSetPropertiesWin2D)]

[!code-cs[BlurAmountWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetBlurAmountWin2D)]


A última etapa é implementar o método [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764794) que realmente processa os dados da imagem.

Usando as APIs Win2D, um **CanvasBitmap** é criado a partir da propriedade [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn930920) do quadro de entrada. Um **CanvasRenderTarget** é criado a partir do **Direct3DSurface** do quadro de saída e um **CanvasDrawingSession** é criado a partir desse destino de renderização. Um novo **GaussianBlurEffect** Win2D é inicializado, usando a propriedade **BlurAmount** que o nosso efeito expõe via [**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986). Por fim, o método **CanvasDrawingSession.DrawImage** é chamado para desenhar o bitmap de entrada para o destino de renderização usando o efeito de desfoque.

[!code-cs[ProcessFrameWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetProcessFrameWin2D)]


## Adicionando efeito personalizado ao seu aplicativo


Para usar o efeito de vídeo do seu aplicativo, você deve adicionar uma referência ao projeto do efeito ao seu aplicativo.

1.  No Gerenciador de Soluções, no projeto do seu aplicativo, clique com o botão direito do mouse em Referências e selecione Adicionar referência...
2.  Expanda a guia Projetos, clique em Solução e marque a caixa de seleção com o nome do projeto do efeito. Neste exemplo, o nome é VideoEffectComponent.
3.  Clique em OK.

### Adicionar o efeito personalizado a um fluxo de vídeo da câmera

Você pode configurar um fluxo de visualização simples da câmera seguindo as etapas do artigo [Acesso de visualização de câmera simples](simple-camera-preview-access.md). As etapas a seguir fornecerão um objeto [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) inicializado que é usado para acessar o fluxo de vídeo da câmera.

Para adicionar o efeito de vídeo personalizado ao fluxo de uma câmera, primeiro crie um novo objeto [**VideoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608055), passando o namespace e o nome de classe do efeito. Em seguida, do objeto **MediaCapture**, chame o método [**AddVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn878035) para adicionar o efeito ao fluxo especificado. Este exemplo usa o valor [**MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640) para especificar se esse efeito deve ser adicionado ao fluxo de inicialização. Se o seu aplicativo der suporte à captura de vídeo, você também poderia usar **MediaStreamType.VideoRecord** para adicionar o efeito ao fluxo de captura. 
            **AddVideoEffect** retorna um objeto [**IMediaExtension**](https://msdn.microsoft.com/library/windows/apps/br240985) que representa o efeito personalizado. Você pode usar o método SetProperties para definir a configuração do efeito.

Depois que o efeito tiver sido adicionado, [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613) é chamado para iniciar o fluxo de visualização.

[!code-cs[AddVideoEffectAsync](./code/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs#SnippetAddVideoEffectAsync)]



### Adicionar o efeito personalizado a um clipe em um MediaComposition

Para obter orientações gerais sobre como criar composições de mídia a partir de clipes de vídeo, consulte [Composições e edição de mídia](media-compositions-and-editing.md). O trecho de código a seguir mostra a criação de uma composição de mídia simples com um efeito de vídeo personalizado. Um objeto [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596) é criado, chamando-se [**CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652607), passando um arquivo de vídeo que foi selecionado pelo usuário com um [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) e o clipe é adicionado a um novo [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646). Em seguida, um novo objeto [**VideoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608055) é criado, passando o namespace e nome de classe do efeito para o construtor. Por fim, a definição do efeito é adicionada à coleção [**VideoEffectDefinitions**](https://msdn.microsoft.com/library/windows/apps/dn652643) do objeto **MediaClip**.


[!code-cs[AddEffectToComposition](./code/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs#SnippetAddEffectToComposition)]


## Tópicos relacionados



            [Acesso de visualização de câmera simples](simple-camera-preview-access.md)
            
          
            [Composições e edição de mídia](media-compositions-and-editing.md)
            
          
            [Documentação do Win2D](http://go.microsoft.com/fwlink/?LinkId=519078)
          
 

 






<!--HONumber=Jun16_HO4-->


