---
Description: Este artigo descreve como criar um componente do Tempo de Execução do Windows que implemente a interface IBasicAudioEffect para permitir que você crie efeitos personalizados para fluxos de áudio.
title: Efeitos de áudio personalizados
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 360faf3f-7e73-4db4-8324-3391f801d827
ms.localizationpriority: medium
ms.openlocfilehash: 6bebf9533ab045822902d44f87f68eec55c11074
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318570"
---
# <a name="custom-audio-effects"></a>Efeitos de áudio personalizados

Este artigo descreve como criar um componente do Tempo de Execução do Windows que implemente a interface [**IBasicAudioEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IBasicAudioEffect) para criar efeitos personalizados para fluxos de áudio. Efeitos personalizados podem ser usados com várias APIs diferentes do Windows Runtime, incluindo [MediaCapture](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture), que fornece acesso à câmera do dispositivo, [**MediaComposition**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaComposition), que permite que você crie composições complexas de clipes de mídia, e [**AudioGraph**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraph), que permite que você monte rapidamente um gráfico de vários nós de entrada, saída e submixagem de áudio.

## <a name="add-a-custom-effect-to-your-app"></a>Adicionar um efeito personalizado ao seu aplicativo


Um efeito de áudio personalizado é definido em uma classe que implementa a interface [**IBasicAudioEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IBasicAudioEffect). Essa classe não pode ser incluída diretamente no projeto do seu aplicativo. Em vez disso, você deve usar um componente do Tempo de Execução do Windows para hospedar sua classe de efeito de áudio.

**Adicionar um componente de tempo de execução do Windows para o efeito de áudio**

1.  No Microsoft Visual Studio, com sua solução aberta, vá para o menu **Arquivo** e selecione **Adicionar-&gt;Novo Projeto**.
2.  Selecione o tipo de projeto **Componente do Tempo de Execução do Windows (Windows Universal)** .
3.  Para este exemplo, chame o projeto de *AudioEffectComponent*. Esse nome será referenciado no código posteriormente.
4.  Clique em **OK**.
5.  O modelo de projeto cria uma classe chamada Class1.cs. No **Gerenciador de Soluções**, clique com botão direito do mouse no ícone de Class1.cs e selecione **Renomear**.
6.  Renomeie o arquivo para *ExampleAudioEffect.cs*. O Visual Studio mostrará um aviso perguntando se você deseja atualizar todas as referências para o novo nome. Clique em **Sim**.
7.  Abra **ExampleAudioEffect.cs** e atualize a definição de classe para implementar a interface [**IBasicAudioEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IBasicAudioEffect).


[!code-cs[ImplementIBasicAudioEffect](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetImplementIBasicAudioEffect)]

Você precisa incluir os seguintes namespaces no arquivo de classe do efeito para acessar todos os tipos usados nos exemplos deste artigo.

[!code-cs[EffectUsing](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetEffectUsing)]

## <a name="implement-the-ibasicaudioeffect-interface"></a>Implementar a interface IBasicAudioEffect

O efeito de áudio deve implementar todos os métodos e propriedades da interface [**IBasicAudioEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IBasicAudioEffect). Esta seção mostra uma implementação simples dessa interface para criar um efeito de eco básico.

### <a name="supportedencodingproperties-property"></a>Propriedade SupportedEncodingProperties

O sistema verifica a propriedade [**SupportedEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicaudioeffect.supportedencodingproperties) para determinar quais propriedades de codificação são compatíveis com o efeito. Observe que se o consumidor do efeito não puder codificar o áudio usando as propriedades que você especificar, o sistema chamará [**Close**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicaudioeffect.close) no efeito e o removerá do pipeline de áudio. Neste exemplo, objetos [**AudioEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.AudioEncodingProperties) são criados e adicionados à lista retornada para dar suporte à codificação mono 44,1 kHz e 48 kHz, float de 32 bits.

[!code-cs[SupportedEncodingProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSupportedEncodingProperties)]

### <a name="setencodingproperties-method"></a>Método SetEncodingProperties

O sistema chama [**SetEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicvideoeffect.setencodingproperties) o efeito para permitir que você conheça as propriedades de codificação do fluxo de áudio no qual o efeito está operando. Para implementar um efeito de eco, este exemplo usa um buffer para armazenar um segundo de dados de áudio. Esse método fornece a oportunidade de inicializar o tamanho do buffer para o número de amostras em um segundo de áudio, com base na taxa de exemplo em que o áudio é codificado. O efeito de atraso também usa um contador de inteiro para acompanhar a posição atual no buffer de atraso. Como **SetEncodingProperties** é chamado sempre que o efeito é adicionado ao pipeline de áudio, este é um bom momento para inicializar esse valor como 0. Você também pode capturar o objeto **AudioEncodingProperties** passado para esse método para usar em outro lugar no seu efeito.

[!code-cs[DeclareEchoBuffer](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetDeclareEchoBuffer)]
[!code-cs[SetEncodingProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSetEncodingProperties)]


### <a name="setproperties-method"></a>Método SetProperties

O método [**SetProperties**](https://docs.microsoft.com/uwp/api/windows.media.imediaextension.setproperties) permite que o aplicativo que está usando o efeito ajuste os parâmetros do efeito. As propriedades são passadas como um mapa de nomes e valores de propriedade [**IPropertySet**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IPropertySet).

[!code-cs[SetProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSetProperties)]

Este exemplo simples misturará a amostra de áudio atual com um valor do buffer de atraso de acordo com o valor da propriedade **Mix**. Uma propriedade é declarada e TryGetValue é usado para obter o valor definido pelo aplicativo de chamada. Se nenhum valor foi definido, um valor padrão de 0,5 será usado. Observe que essa propriedade é somente leitura. O valor da propriedade deve ser definido usando **SetProperties**.

[!code-cs[MixProperty](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetMixProperty)]

### <a name="processframe-method"></a>Método ProcessFrame

O método [**ProcessFrame**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicaudioeffect.processframe) é onde o efeito modifica os dados de áudio do stream. O método é chamado uma vez por quadro e um objeto [**ProcessAudioFrameContext**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.ProcessAudioFrameContext) é passado. Este objeto contém um objeto [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame) de entrada que contém o quadro de entrada a ser processado e um objeto **AudioFrame** de saída no qual você grava os dados de áudio que serão passados para o resto do pipeline de áudio. Um quadro de áudio é um buffer de amostras de áudio que representa uma pequena fatia de dados de áudio.

Acessar o buffer de dados de um **AudioFrame** requer interoperabilidade COM. Portanto, você deve incluir o namespace **System.Runtime.InteropServices** no arquivo de classe de efeito e, em seguida, adicionar o seguinte código dentro do namespace para que o efeito importe a interface para acessar o buffer de áudio.

[!code-cs[ComImport](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetComImport)]

> [!NOTE]
> Como essa técnica acessa um buffer de imagem nativo não gerenciado, você precisará configurar seu projeto para permitir código não seguro.
> 1.  No Gerenciador de Soluções, clique com o botão direito do mouse no projeto AudioEffectComponent e selecione **Propriedades**.
> 2.  Selecione a guia **Compilar**.
> 3.  Marque a caixa de seleção **Permitir código não seguro**.

 

Agora você pode adicionar a implementação do método **ProcessFrame** ao efeito. Primeiro, esse método obtém um objeto [**AudioBuffer**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioBuffer) a partir dos quadros de áudio de entrada e de saída. Observe que o quadro de saída é aberto para gravação e o de entrada para leitura. Em seguida, um [**IMemoryBufferReference**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IMemoryBufferReference) é obtido para cada buffer, chamando-se [**CreateReference**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer.createreference). Em seguida, o buffer de dados real é obtido, transmitindo os objetos **IMemoryBufferReference** como a interface de interoperabilidade COM definida acima, **IMemoryByteAccess**, e chamando **GetBuffer**.

Agora que os buffers de dados foram obtidos, você pode ler o buffer de entrada e gravar no buffer de saída. Para cada exemplo do inputbuffer, o valor é obtido e multiplicado por 1 - **Mix** para definir o valor de sinal seco do efeito. Em seguida, uma amostra é recuperada da posição atual no buffer de eco e multiplicada por **Mix** para definir o valor úmido do efeito. O exemplo de saída é definido como a soma dos valores úmido e seco. Por fim, cada amostra de entrada é armazenada no buffer de eco e o índice atual de amostra é incrementado.

[!code-cs[ProcessFrame](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetProcessFrame)]



### <a name="close-method"></a>Método Close

O sistema irá chamar o [ **feche** ](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicaudioeffect.close) [ **fechar** ](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicaudioeffect.close) método em sua classe quando o efeito deve ser desligado. Você deve usar esse método para descartar quaisquer recursos que você criou. O argumento para o método é um [**MediaEffectClosedReason**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.MediaEffectClosedReason) que permite que você saiba se o efeito foi fechado normalmente, se ocorreu um erro ou se o efeito não é compatível com o formato de codificação necessário.

[!code-cs[Close](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetClose)]

### <a name="discardqueuedframes-method"></a>Método DiscardQueuedFrames

O método [**DiscardQueuedFrames**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicvideoeffect.discardqueuedframes) é chamado quando o efeito deve ser redefinido. Um cenário típico para isso é se o efeito armazena quadros processados anteriormente para serem usados no processamento do quadro atual. Quando esse método é chamado, você deve descartar o conjunto de quadros anteriores que foram salvos. Esse método possa ser usado para restaurar qualquer estado relacionado ao quadros anteriores, não apenas quadros de áudio acumulados.

[!code-cs[DiscardQueuedFrames](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetDiscardQueuedFrames)]

### <a name="timeindependent-property"></a>Propriedade TimeIndependent

A propriedade TimeIndependent [**TimeIndependent**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicvideoeffect.timeindependent) permite que o sistema saiba se o efeito não requer tempo uniforme. Quando definida como true, o sistema pode usar otimizações que aprimorem o desempenho do efeito.

[!code-cs[TimeIndependent](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetTimeIndependent)]

### <a name="useinputframeforoutput-property"></a>Propriedade UseInputFrameForOutput

Defina a propriedade [**UseInputFrameForOutput**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicaudioeffect.useinputframeforoutput) como **true** para dizer ao sistema que o efeito gravará sua saída no buffer de áudio do [**InputFrame**](https://docs.microsoft.com/uwp/api/windows.media.effects.processaudioframecontext.inputframe) do [**ProcessAudioFrameContext**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.ProcessAudioFrameContext) transmitido para [**ProcessFrame**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicaudioeffect.processframe) em vez de gravar no [**OutputFrame**](https://docs.microsoft.com/uwp/api/windows.media.effects.processaudioframecontext.outputframe). 

[!code-cs[UseInputFrameForOutput](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetUseInputFrameForOutput)]


## <a name="adding-your-custom-effect-to-your-app"></a>Adicionando efeito personalizado ao seu aplicativo


Para usar o efeito de áudio do seu aplicativo, você deve adicionar uma referência ao projeto do efeito ao seu aplicativo.

1.  No Gerenciador de Soluções, no projeto do seu aplicativo, clique com o botão direito do mouse em **Referências** e selecione **Adicionar referência**.
2.  Expanda a guia **Projetos**, selecione **Solução** e marque a caixa de seleção com o nome do projeto do efeito. Neste exemplo, o nome é *AudioEffectComponent*.
3.  Clique em **OK**.

Se sua classe de efeito de áudio declarada for um namespace diferente, inclua esse namespace no seu arquivo de código.

[!code-cs[UsingAudioEffectComponent](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUsingAudioEffectComponent)]

### <a name="add-your-custom-effect-to-an-audiograph-node"></a>Adicionar o efeito personalizado a um nó AudioGraph
Para obter informações gerais sobre como usar os gráficos de áudio, consulte [Gráficos de áudio](audio-graphs.md). O snippet de código a seguir mostra como adicionar o efeito de eco de exemplo mostrado neste artigo para um nó de gráfico de áudio. Primeiro, um [**PropertySet**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.PropertySet) é criado e um valor para a propriedade **Mix**, definida pelo efeito, é definida. Em seguida, o construtor [**AudioEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.AudioEffectDefinition) é chamado, transmitindo o nome completo da classe do tipo de efeito personalizado e o conjunto de propriedades. Por fim, a definição de efeito é adicionada à propriedade [**EffectDefinitions**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.effectdefinitions) de um [**FileInputNode**](https://docs.microsoft.com/uwp/api/windows.media.audio.createaudiofileinputnoderesult.fileinputnode) existente, fazendo com que o áudio emitido seja processado pelo efeito personalizado. 

[!code-cs[AddCustomEffect](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddCustomEffect)]

Depois que ele foi adicionado a um nó, o efeito personalizado poderá ser desabilitado chamando [**DisableEffectsByDefinition**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.disableeffectsbydefinition) e transmitindo o objeto **AudioEffectDefinition**. Para obter mais informações sobre como usar os gráficos de áudio em seu aplicativo, consulte [AudioGraph](audio-graphs.md).

### <a name="add-your-custom-effect-to-a-clip-in-a-mediacomposition"></a>Adicionar o efeito personalizado a um clipe em um MediaComposition

O snippet de código a seguir demonstra como adicionar o efeito de áudio personalizado a um clipe de vídeo e uma faixa de áudio em segundo plano em uma composição de mídia. Para obter orientações gerais sobre como criar composições de mídia a partir de clipes de vídeo e adicionar faixas de áudio em segundo plano, consulte [Composições e edição de mídia](media-compositions-and-editing.md).

[!code-cs[AddCustomAudioEffect](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddCustomAudioEffect)]



## <a name="related-topics"></a>Tópicos relacionados
* [Acesso de visualização da câmera simples](simple-camera-preview-access.md)
* [Composições de mídia e edição](media-compositions-and-editing.md)
* [Documentação de Win2D](https://go.microsoft.com/fwlink/p/?LinkId=519078)
* [Reprodução de mídia](media-playback.md)

 



