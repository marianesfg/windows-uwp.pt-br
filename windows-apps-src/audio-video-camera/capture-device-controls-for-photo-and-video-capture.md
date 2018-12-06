---
ms.assetid: 831123A7-1F40-4B74-AE9F-69AC9883B4AD
description: Este artigo mostra como usar controles de dispositivo manuais para habilitar cenários de captura de fotos e vídeos avançados, incluindo estabilização de imagem óptica e zoom suave.
title: Controles manuais da câmera para a captura de fotos e vídeos
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 303cbd5e87db773324cd98447df6d99dc6de5a0c
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8744567"
---
# <a name="manual-camera-controls-for-photo-and-video-capture"></a>Controles manuais da câmera para a captura de fotos e vídeos



Este artigo mostra como usar controles de dispositivo manuais para habilitar cenários de captura de fotos e vídeos avançados, incluindo estabilização de imagem óptica e zoom suave.

Os controles discutidos neste artigo são todos adicionados ao seu aplicativo usando o mesmo padrão. Primeiro, verifique se o controle tem suporte no dispositivo atual em que seu aplicativo está sendo executado. Se o controle tiver suporte, defina o modo desejado para o controle. Normalmente, se um determinado controle não tiver suporte no dispositivo atual, você deverá desabilitar ou ocultar o elemento da interface do usuário que permite ao usuário habilitar o recurso.

O código neste artigo foi adaptado da [amostra do SDK de Controles Manuais de Câmera](https://go.microsoft.com/fwlink/?linkid=845228). Você pode baixar a amostra para ver o código usado no contexto ou usar a amostra como ponto de partida para seu próprio aplicativo.

> [!NOTE]
> Este artigo se baseia em conceitos e códigos discutidos em [Captura básica de fotos, áudio e vídeo com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), que descreve as etapas para implementar uma captura básica de fotos e vídeos. Recomendamos que você se familiarize com o padrão de captura de mídia básica neste artigo antes de passar para cenários de captura mais avançados. O código neste artigo presume que seu aplicativo já tenha uma instância de MediaCapture inicializada corretamente.

Todas as APIs de controle de dispositivo discutidas neste artigo são membros do namespace [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902).

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

## <a name="exposure"></a>Exposição

O [**ExposureControl**](https://msdn.microsoft.com/library/windows/apps/dn278910) permite definir a velocidade do obturador usada durante a captura de foto ou vídeo.

Este exemplo usa um controle [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) para ajustar o valor de exposição atual e uma caixa de seleção para alternar o ajuste automático da exposição.

[!code-xml[ExposureXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetExposureXAML)]

Confirme se o dispositivo de captura atual dá suporte ao **ExposureControl** verificando a propriedade [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297710). Havendo suporte para o controle, você pode mostrar e habilitar a interface do usuário para esse recurso. Defina o estado de marcação da caixa de seleção para indicar se o ajuste automático da exposição está atualmente ativo para o valor da propriedade [**Auto**](https://msdn.microsoft.com/library/windows/apps/dn278911).

O valor da exposição deve estar dentro do intervalo aceito pelo dispositivo e deve ser um incremento do tamanho da etapa com suporte. Obtenha os valores aceitos para o dispositivo atual verificando as propriedades [**Min**](https://msdn.microsoft.com/library/windows/apps/dn278919), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn278914) e [**Step**](https://msdn.microsoft.com/library/windows/apps/dn278930), que são usadas para definir as propriedades correspondentes do controle deslizante.

Defina o valor do controle deslizante como o valor atual de **ExposureControl** depois de cancelar o registro do manipulador de eventos [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) para que o evento não seja disparado quando o valor for definido.

[!code-cs[ExposureControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureControl)]

No manipulador de eventos **ValueChanged**, obtenha o valor atual do controle e defina o valor da exposição chamando [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn278927).

[!code-cs[ExposureSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureSlider)]

No manipulador de eventos **CheckedChanged** da caixa de seleção de exposição automática, ative ou desative o ajuste automático da exposição chamando [**SetAutoAsync**](https://msdn.microsoft.com/library/windows/apps/dn278920) e transmitindo um valor booliano.

[!code-cs[ExposureCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureCheckBox)]

> [!IMPORTANT]
> Somente há suporte para o modo de exposição automática enquanto o fluxo de visualização estiver em execução. Verifique se o fluxo de visualização está em execução antes de ativar a exposição automática.

## <a name="exposure-compensation"></a>Compensação de exposição

O [**ExposureCompensationControl**](https://msdn.microsoft.com/library/windows/apps/dn278897) permite definir a compensação de exposição usada durante a captura de fotos ou vídeos.

Este exemplo usa um controle [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) para ajustar o valor atual da compensação de exposição.

[!code-xml[EvXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetEvXAML)]

Confirme se o dispositivo de captura atual dá suporte ao **ExposureCompensationControl** verificando a propriedade [Supported](supported-codecs.md). Havendo suporte para o controle, você pode mostrar e habilitar a interface do usuário para esse recurso.

O valor da compensação de exposição deve estar dentro do intervalo aceito pelo dispositivo e deve ser um incremento do tamanho da etapa com suporte. Obtenha os valores aceitos para o dispositivo atual verificando as propriedades [**Min**](https://msdn.microsoft.com/library/windows/apps/dn278901), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn278899) e [**Step**](https://msdn.microsoft.com/library/windows/apps/dn278904), que são usadas para definir as propriedades correspondentes do controle deslizante.

Defina o valor do controle deslizante como o valor atual de **ExposureCompensationControl** depois de cancelar o registro do manipulador de eventos [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) para que o evento não seja disparado quando o valor for definido.

[!code-cs[EvControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEvControl)]

No manipulador de eventos **ValueChanged**, obtenha o valor atual do controle e defina o valor da exposição chamando [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn278903).

[!code-cs[EvValueChanged](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEvValueChanged)]

## <a name="flash"></a>Flash

O [**FlashControl**](https://msdn.microsoft.com/library/windows/apps/dn297725) permite que você habilite ou desabilite o flash ou habilite o flash automático, com o qual o sistema determina dinamicamente se deve ou não usar o flash. Esse controle também permite que você habilite a redução automática de olhos vermelhos em dispositivos com suporte para esse recurso. Todas essas configurações se aplicam à captura de fotos. O [**TorchControl**](https://msdn.microsoft.com/library/windows/apps/dn279077) é um controle separado para ativar ou desativar a lanterna para captura de vídeos.

Este exemplo usa um conjunto de botões de opção para permitir que o usuário alterne entre as configurações de flash ativado, desativado e automático. Uma caixa de seleção também é fornecida para permitir a alternância entre a redução de olhos vermelhos e a lanterna de vídeo.

[!code-xml[FlashXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetFlashXAML)]

Verifique se o dispositivo de captura atual dá suporte ao **FlashControl** verificando a propriedade [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297837). Havendo suporte para o controle, você pode mostrar e habilitar a interface do usuário para esse recurso. Se houver suporte para **FlashControl**, a redução automática de olhos vermelhos poderá ou não ser compatível. Portanto, verifique a propriedade [**RedEyeReductionSupported**](https://msdn.microsoft.com/library/windows/apps/dn297766) antes de habilitar a interface do usuário. Como **TorchControl** é separado do controle de flash, você também deve verificar sua propriedade [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn279081) antes de usá-lo.

No manipulador de eventos [**Checked**](https://msdn.microsoft.com/library/windows/apps/br209796) para cada um dos botões de opção de flash, habilite ou desabilite a configuração de flash correspondente apropriada. Observe que, para definir o flash de forma que ele sempre seja usado, você deve definir a propriedade [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn297733) como true e a propriedade [**Auto**](https://msdn.microsoft.com/library/windows/apps/dn297728) como false.

[!code-cs[FlashControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFlashControl)]

[!code-cs[FlashRadioButtons](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFlashRadioButtons)]

No manipulador da caixa de seleção de redução de olhos vermelhos, defina a propriedade [**RedEyeReduction**](https://msdn.microsoft.com/library/windows/apps/dn297758) como o valor apropriado.

[!code-cs[RedEye](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetRedEye)]

Por fim, no manipulador da caixa de seleção da lanterna de vídeo, defina a propriedade [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn279078) como o valor apropriado.

[!code-cs[Torch](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTorch)]

> [!NOTE] 
>  Em alguns dispositivos, o lanterna não emitirá luz, mesmo se [**TorchControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn279078) for definido como true, a menos que o dispositivo tenha um fluxo de visualização em execução e esteja ativamente capturando vídeo. A ordem recomendada das operações é ativar a visualização de vídeo, ativar a lanterna definindo **Enabled** como true e depois iniciar a captura de vídeo. Em alguns dispositivos, a lanterna acende depois que a visualização é iniciada. Em outros dispositivos, o lanterna não pode acender até que captura de vídeo seja iniciada.

## <a name="focus"></a>Foco

Três métodos diferentes comumente usados para ajustar o foco da câmera são aceitos pelo objeto [**FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn297788): foco automático contínuo, tocar para focalizar e foco manual. Um aplicativo de câmera pode oferecer suporte a todos esses três métodos, mas para facilitar a leitura, este artigo descreve cada técnica separadamente. Esta seção também explica como habilitar a luz auxiliar de foco.

### <a name="continuous-autofocus"></a>Foco automático contínuo

Habilitar o foco automático contínuo instrui a câmera para ajustar o foco dinamicamente para tentar manter o sujeito da foto ou vídeo em foco. Este exemplo usa um botão de opção para ativar e desativar o foco automático contínuo.

[!code-xml[CAFXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetCAFXAML)]

Confirme se o dispositivo de captura atual dá suporte ao **FocusControl** verificando a propriedade [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297785). Em seguida, veja se há suporte para o foco automático verificando a lista [**SupportedFocusModes**](https://msdn.microsoft.com/library/windows/apps/dn608079) para ver se ela contém o valor [**FocusMode.Continuous**](https://msdn.microsoft.com/library/windows/apps/dn608084) e, se contiver, mostrar o botão de opção de foco automático contínuo.

[!code-cs[CAF](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetCAF)]

No manipulador de eventos [**Checked**](https://msdn.microsoft.com/library/windows/apps/br209796) do botão de opção de foco automático contínuo, use a propriedade [**VideoDeviceController.FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn279091) para obter uma instância do controle. Chame [**UnlockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608081) para desbloquear o controle caso seu aplicativo tenha chamado [**LockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608075) anteriormente para habilitar um dos modos de foco.

Crie um novo objeto [**FocusSettings**](https://msdn.microsoft.com/library/windows/apps/dn608085) e defina a propriedade [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn608090) como **Continuous**. Defina a propriedade [**AutoFocusRange**](https://msdn.microsoft.com/library/windows/apps/dn608086) como um valor apropriado para o seu cenário de aplicativo ou como um valor selecionado pelo usuário na sua interface do usuário. Transmita o objeto **FocusSettings** para o método [**Configure**](https://msdn.microsoft.com/library/windows/apps/dn608067) e depois chame [**FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794) para iniciar um foco automático contínuo.

[!code-cs[CafFocusRadioButton](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetCafFocusRadioButton)]

> [!IMPORTANT]
> O modo de foco automático é aceito somente enquanto o fluxo de visualização está em execução. Verifique se o fluxo de visualização está em execução antes de ativar o foco automático contínuo.

### <a name="tap-to-focus"></a>Tocar para focalizar

A técnica de tocar para focalizar usa o [**FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn297788) e o [**RegionsOfInterestControl**](https://msdn.microsoft.com/library/windows/apps/dn279064) para especificar uma sub-região do quadro de captura na qual o dispositivo de captura deve focalizar. A região de foco é determinada pelo usuário ao tocar na tela que exibe o fluxo de visualização.

Este exemplo usa um botão de opção para habilitar e desabilitar o modo "tocar para focalizar".

[!code-xml[TapFocusXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetTapFocusXAML)]

Confirme se o dispositivo de captura atual dá suporte ao **FocusControl** verificando a propriedade [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297785). O **RegionsOfInterestControl** deve ter suporte e deve dar suporte a pelo menos uma região para usar essa técnica. Verifique as propriedades [**AutoFocusSupported**](https://msdn.microsoft.com/library/windows/apps/dn279066) e [**MaxRegions**](https://msdn.microsoft.com/library/windows/apps/dn279069) para determinar se o botão de opção de tocar para focalizar deve ser mostrado ou ocultado.

[!code-cs[TapFocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocus)]

No manipulador de eventos [**Checked**](https://msdn.microsoft.com/library/windows/apps/br209796) do botão de opção de tocar para focalizar, use a propriedade [**VideoDeviceController.FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn279091) para obter uma instância do controle. Chame [**LockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608075) para bloquear o controle caso seu aplicativo tenha sido chamado anteriormente [**UnlockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608081) para habilitar o foco automático contínuo e aguarde até que o usuário toque na tela para alterar o foco.

[!code-cs[TapFocusRadioButton](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocusRadioButton)]

Este exemplo foca em uma região quando o usuário toca na tela e, em seguida, remove o foco dessa região quando o usuário toca novamente, como uma alternância. Use uma variável booliana para rastrear o estado de alternância atual.

[!code-cs[IsFocused](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsFocused)]

A próxima etapa é escutar o evento quando o usuário tocar na tela, manipulando o evento [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985) do [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) que está exibindo atualmente o fluxo de visualização da captura. Se a câmera não estiver atualmente em visualização, ou se o modo de toque para foco estiver desabilitado, retorne a partir do manipulador sem fazer nada.

Se a variável de rastreamento *\_isFocused* for alternada para falso e se a câmera não estiver em processo de foco (determinado pela propriedade [**FocusState**](https://msdn.microsoft.com/library/windows/apps/dn608074) do **FocusControl**), inicie o processo de tocar para focalizar. Obtenha a posição de toque do usuário dos argumentos de evento passados para o manipulador. Este exemplo também aproveita a oportunidade para selecionar o tamanho da região que será focalizada. Neste caso, o tamanho é 1/4 da menor dimensão do elemento de captura. Passe a posição do toque e o tamanho da região para o método auxiliar **TapToFocus** que é definido na próxima seção.

Se a alternância de *\_isFocused* estiver definida como true, o toque do usuário deverá limpar o foco da região anterior. Isso é feito no método auxiliar **TapUnfocus**, mostrado abaixo.

[!code-cs[TapFocusPreviewControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocusPreviewControl)]

No método auxiliar **TapToFocus**, primeiro defina a alternância de *\_isFocused* como true para que o próximo toque na tela libere o foco da região tocada.

A próxima tarefa nesse método auxiliar é determinar o retângulo dentro do fluxo de visualização será atribuído ao controle de foco. Isso exige duas etapas. A primeira etapa é determinar o retângulo que o fluxo de visualização ocupa dentro do controle [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278). Isso depende das dimensões do fluxo de visualização e da orientação do dispositivo. O método auxiliar **GetPreviewStreamRectInControl**, mostrado no final desta seção, executa essa tarefa e retorna o retângulo que contém o fluxo de visualização.

A próxima tarefa em **TapToFocus** é converter o local de toque e o tamanho do retângulo de foco desejado, que foram determinados no manipulador de eventos **CaptureElement.Tapped**, em coordenadas no fluxo de captura. O método auxiliar **ConvertUiTapToPreviewRect**, mostrado posteriormente nesta seção, executa essa conversão e retorna o retângulo, nas coordenadas do fluxo de captura, onde o foco será solicitado.

Agora que o retângulo de destino foi obtido, crie um novo objeto [**RegionOfInterest**](https://msdn.microsoft.com/library/windows/apps/dn279058), definindo a propriedade [**Bounds**](https://msdn.microsoft.com/library/windows/apps/dn279062) como o retângulo de destino obtido nas etapas anteriores.

Obtenha o [**FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn297788) do dispositivo de captura. Criar um novo objeto [**FocusSettings**](https://msdn.microsoft.com/library/windows/apps/dn608085) e defina [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn608076) e [**AutoFocusRange**](https://msdn.microsoft.com/library/windows/apps/dn608086) como seus valores desejados depois de verificar se eles têm suporte por **FocusControl**. Chame [**Configure**](https://msdn.microsoft.com/library/windows/apps/dn608067) no **FocusControl** para ativar as configurações e sinalizar o dispositivo para começar a focalizar a região especificada.

Em seguida, obtenha o [**RegionsOfInterestControl**](https://msdn.microsoft.com/library/windows/apps/dn279064) do dispositivo de captura e chame [**SetRegionsAsync**](https://msdn.microsoft.com/library/windows/apps/dn279070) para definir a região ativa. Várias regiões de interesse podem ser definidas em dispositivos que permitem isso, mas este exemplo define apenas uma região.

Por fim, chame [**FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794) no **FocusControl** para iniciar a focalização.

> [!IMPORTANT]
> Ao implementar tocar para focalizar, a ordem das operações é importante. Você deve chamar essas APIs na seguinte ordem:
>
> 1. [**FocusControl.Configure**](https://msdn.microsoft.com/library/windows/apps/dn608067)
> 2. [**RegionsOfInterestControl.SetRegionsAsync**](https://msdn.microsoft.com/library/windows/apps/dn279070)
> 3. [**FocusControl.FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794)

[!code-cs[TapToFocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapToFocus)]

No método auxiliar **TapUnfocus**, obtenha o **RegionsOfInterestControl** e chame [**ClearRegionsAsync**](https://msdn.microsoft.com/library/windows/apps/dn279068) para limpar a região que foi registrada com o controle no método auxiliar **TapToFocus**. Em seguida, obtenha o **FocusControl** e chame [**FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794) para fazer com que o dispositivo focalize novamente sem uma região de interesse.

[!code-cs[TapUnfocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapUnfocus)]

O método auxiliar **GetPreviewStreamRectInControl** usa a resolução do fluxo de visualização e a orientação do dispositivo para determinar o retângulo dentro do elemento de visualização que contém o fluxo de visualização, cortando qualquer preenchimento em letterbox que o controle possa fornecer para manter a taxa de proporção do fluxo. Este método usa variáveis membros da classe definidas no código de exemplo de captura de mídia básica encontrado em [Captura básica de fotos, aúdio e vídeo com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

[!code-cs[GetPreviewStreamRectInControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetGetPreviewStreamRectInControl)]

O método auxiliar **ConvertUiTapToPreviewRect** assume como argumentos o local do evento de toque, o tamanho desejado da região de foco e o retângulo que contém o fluxo de visualização obtido do método auxiliar **GetPreviewStreamRectInControl**. Esse método usa esses valores e orientação atual do dispositivo para calcular o retângulo dentro do fluxo de visualização que contém a região desejada. Mais uma vez, esse método usa variáveis membros da classe definidas no código de exemplo de captura de mídia básica encontrado em [Capturar fotos e vídeos com o MediaCapture](capture-photos-and-video-with-mediacapture.md).

[!code-cs[ConvertUiTapToPreviewRect](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetConvertUiTapToPreviewRect)]

### <a name="manual-focus"></a>Foco manual

A técnica de foco manual usa um controle **Slider** para definir a intensidade de foco atual do dispositivo de captura. Um botão de opção é usado para ativar e desativar o foco manual.

[!code-xml[ManualFocusXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetManualFocusXAML)]

Confirme se o dispositivo de captura atual dá suporte ao **FocusControl** verificando a propriedade [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297837). Havendo suporte para o controle, você pode mostrar e habilitar a interface do usuário para esse recurso.

O valor do foco deve estar dentro do intervalo aceito pelo dispositivo e deve ser um incremento do tamanho da etapa com suporte. Obtenha os valores aceitos para o dispositivo atual verificando as propriedades [**Min**](https://msdn.microsoft.com/library/windows/apps/dn297808), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn297802) e [**Step**](https://msdn.microsoft.com/library/windows/apps/dn297833), que são usadas para definir as propriedades correspondentes do controle deslizante.

Defina o valor do controle deslizante como o valor atual de **FocusControl** depois de cancelar o registro do manipulador de eventos [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) para que o evento não seja disparado quando o valor for definido.

[!code-cs[Focus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocus)]

No manipulador de eventos **Checked** para o botão de opção de foco manual, obtenha o objeto **FocusControl** e chame [**LockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608075) caso seu aplicativo tenha desbloqueado anteriormente o foco com uma chamada para [**UnlockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608081).

[!code-cs[ManualFocusChecked](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetManualFocusChecked)]

No manipulador de eventos **ValueChanged** do controle deslizante de foco manual, obtenha o valor atual do controle e defina o valor do foco chamando [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn297828).

[!code-cs[FocusSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusSlider)]

### <a name="enable-the-focus-light"></a>Habilitar a luz de foco

Em dispositivos com suporte, você pode habilitar uma luz auxiliar de foco para ajudar no foco do dispositivo. Este exemplo usa uma caixa de seleção para habilitar ou desabilitar a luz auxiliar de foco.

[!code-xml[FocusLightXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetFocusLightXAML)]

Verifique se o dispositivo de captura atual dá suporte ao **FlashControl** verificando a propriedade [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297785). Também verifique o [**AssistantLightSupported**](https://msdn.microsoft.com/library/windows/apps/dn608066) para saber se também há suporte para a luz auxiliar. Havendo suporte para ambos, você pode mostrar e habilitar a interface do usuário para esse recurso.

[!code-cs[FocusLight](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusLight)]

No manipulador de eventos **CheckedChanged**, obtenha o objeto [**FlashControl**](https://msdn.microsoft.com/library/windows/apps/dn297725) dos dispositivos de captura. Defina a propriedade [**AssistantLightEnabled**](https://msdn.microsoft.com/library/windows/apps/dn608065) para habilitar ou desabilitar a luz de foco.

[!code-cs[FocusLightCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusLightCheckBox)]

## <a name="iso-speed"></a>Velocidade ISO

O [**IsoSpeedControl**](https://msdn.microsoft.com/library/windows/apps/dn297850) permite definir a velocidade ISO usada durante a captura de fotos ou vídeos.

Este exemplo usa um controle [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) para ajustar o valor de compensação da exposição atual e uma caixa de seleção para alternar o ajuste automático da velocidade ISO.

[!code-xml[IsoXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetIsoXAML)]

Confirme se o dispositivo de captura atual dá suporte ao **IsoSpeedControl** verificando a propriedade [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297869). Havendo suporte para o controle, você pode mostrar e habilitar a interface do usuário para esse recurso. Defina o estado de marcação da caixa de seleção para indicar se o ajuste automático da velocidade ISO está atualmente ativo para o valor da propriedade [**Auto**](https://msdn.microsoft.com/library/windows/apps/dn608093).

O valor da velocidade ISO deve estar dentro do intervalo aceito pelo dispositivo e deve ser um incremento do tamanho da etapa com suporte. Obtenha os valores aceitos para o dispositivo atual verificando as propriedades [**Min**](https://msdn.microsoft.com/library/windows/apps/dn608095), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn608094) e [**Step**](https://msdn.microsoft.com/library/windows/apps/dn608129), que são usadas para definir as propriedades correspondentes do controle deslizante.

Defina o valor do controle deslizante como o valor atual de **IsoSpeedControl** depois de cancelar o registro do manipulador de eventos [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) para que o evento não seja disparado quando o valor for definido.

[!code-cs[IsoControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoControl)]

No manipulador de eventos **ValueChanged**, obtenha o valor atual do controle e defina o valor da velocidade ISO chamando [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn608128).

[!code-cs[IsoSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoSlider)]

No manipulador de eventos **CheckedChanged** da caixa de seleção de velocidade ISO automática, ative o ajuste automático da velocidade ISO chamando [**SetAutoAsync**](https://msdn.microsoft.com/library/windows/apps/dn608127). Desative o ajuste automático da velocidade ISO chamando [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn608128) e transmitindo o valor atual do controle deslizante.

[!code-cs[IsoCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoCheckBox)]

## <a name="optical-image-stabilization"></a>Estabilização de imagem ótica

A OIS (estabilização de imagem ótica) estabiliza um fluxo de vídeo capturado manipulando mecanicamente o hardware do dispositivo de captura, o que pode fornecer um resultado superior ao da estabilização digital. Em dispositivos que não dão suporte à OIS, você pode usar o VideoStabilizationEffect para executar a estabilização digital nos vídeos capturados. Para obter mais informações, consulte [Efeitos para captura de vídeo](effects-for-video-capture.md).

Determine se a OIS tem suporte no dispositivo atual, verificando a propriedade [**OpticalImageStabilizationControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926689).

O controle OIS dá suporte a três modos: ativado, desativado e automático, o que significa que o dispositivo determina dinamicamente se a OIS melhoraria a captura de mídia e, em caso afirmativo, habilita a OIS. Para determinar se um modo específico tem suporte em um dispositivo, verifique se a coleção [**OpticalImageStabilizationControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926690) contém o modo desejado.

Habilite ou desabilite a OIS definindo o [**OpticalImageStabilizationControl.Mode**](https://msdn.microsoft.com/library/windows/apps/dn926691) como o modo desejado.

[!code-cs[SetOpticalImageStabilizationMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetOpticalImageStabilizationMode)]

## <a name="powerline-frequency"></a>Frequência de Powerline
Alguns dispositivos de câmera oferecem suporte ao processamento anticintilação que depende de saber a frequência de CA de powerlines no ambiente atual. Alguns dispositivos oferecem suporte à determinação automática da frequência de powerline, enquanto outras exigem que a frequência seja definida manualmente. O exemplo de código a seguir mostra como determinar o suporte à frequência de powerline no dispositivo e, se necessário, como definir a frequência manualmente. 

Primeiro, chame o método **VideoDeviceController** [**TryGetPowerlineFrequency**](https://msdn.microsoft.com/library/windows/apps/br206898), passando um parâmetro de saída do tipo [**PowerlineFrequency**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.PowerlineFrequency); se essa chamada falhar, o controle de frequência de powerline não terá suporte no dispositivo atual. Se houver suporte para o recurso, você poderá determinar se o modo automático está disponível no dispositivo tentando definir o modo automático. Fazer isso chamando [**TrySetPowerlineFrequency**](https://msdn.microsoft.com/library/windows/apps/br206899) e passando o valor **automaticamente**. Se a chamada tiver êxito, significa que sua frequência de powerline automática é compatível. Se o controlador de frequência de powerline for compatível com o dispositivo, mas a detecção automática de frequência não for, você ainda poderá definir a frequência manualmente usando **TrySetPowerlineFrequency**. Neste exemplo, **MyCustomFrequencyLookup** é um método personalizado que você implementa para determinar a frequência correta para o local atual do dispositivo. 

[!code-cs[PowerlineFrequency](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetPowerlineFrequency)]

## <a name="white-balance"></a>Proporção de branco

O [**WhiteBalanceControl**](https://msdn.microsoft.com/library/windows/apps/dn279104) permite definir a proporção de branco usada durante a captura de fotos ou vídeos.

Este exemplo usa um controle [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348) para selecionar uma das predefinições de temperatura de cor internas e um controle [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) para o ajuste manual da proporção de branco.

[!code-xml[WhiteBalanceXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetWhiteBalanceXAML)]

Confirme se o dispositivo de captura atual dá suporte ao **WhiteBalanceControl** verificando a propriedade [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn279120). Havendo suporte para o controle, você pode mostrar e habilitar a interface do usuário para esse recurso. Defina os itens da caixa de combinação com os valores da enumeração [**ColorTemperaturePreset**](https://msdn.microsoft.com/library/windows/apps/dn278894). Defina também o item selecionado como o valor atual da propriedade [**Preset**](https://msdn.microsoft.com/library/windows/apps/dn279110).

Para controle manual, o valor da proporção de branco deve estar dentro do intervalo permitido pelo dispositivo e deve ser um incremento do tamanho da etapa com suporte. Obtenha os valores aceitos para o dispositivo atual verificando as propriedades [**Min**](https://msdn.microsoft.com/library/windows/apps/dn279109), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn279107) e [**Step**](https://msdn.microsoft.com/library/windows/apps/dn279119), que são usadas para definir as propriedades correspondentes do controle deslizante. Antes de habilitar o controle manual, certifique-se de que o intervalo entre os valores mínimos e máximo com suporte seja maior que o tamanho da etapa. Caso contrário, o controle manual não terá suporte no dispositivo atual.

Defina o valor do controle deslizante como o valor atual de **WhiteBalanceControl** depois de cancelar o registro do manipulador de eventos [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) para que o evento não seja disparado quando o valor for definido.

[!code-cs[WhiteBalance](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalance)]

No manipulador de eventos [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) da caixa de combinação de predefinições de temperatura de cor, obtenha a predefinição atualmente selecionada e defina o valor do controle chamando [**SetPresetAsync**](https://msdn.microsoft.com/library/windows/apps/dn279113). Se o valor predefinido selecionado não for **Manual**, desabilite o controle deslizante de proporção de branco manual.

[!code-cs[WhiteBalanceComboBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalanceComboBox)]

No manipulador de eventos **ValueChanged**, obtenha o valor atual do controle e defina o valor da proporção de branco chamando [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn278927).

[!code-cs[WhiteBalanceSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalanceSlider)]

> [!IMPORTANT]
> O ajuste da proporção de branco só é permitido enquanto o fluxo de visualização estiver em execução. Verifique se o fluxo de visualização está em execução antes de definir o valor ou a predefinição da proporção de branco.

> [!IMPORTANT]
> O valor predefinido **ColorTemperaturePreset.Auto** instrui o sistema a ajustar automaticamente o nível de proporção de branco. Para alguns cenários, como a captura de uma sequência de fotos em que os níveis de proporção de branco devem ser iguais para cada quadro, convém bloquear o controle no valor automático atual. Para fazer isso, chame [**SetPresetAsync**](https://msdn.microsoft.com/library/windows/apps/dn279113) e especifique a predefinição **Manual** e não defina um valor no controle usando [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn279114). Isso fará com que o dispositivo bloqueie o valor atual. Não tente ler o valor atual do controle atual e depois transmitir o valor retornado para **SetValueAsync**, pois não há garantia de que esse valor seja correto.

## <a name="zoom"></a>Zoom

O [**ZoomControl**](https://msdn.microsoft.com/library/windows/apps/dn608149) permite definir o nível de zoom usado durante a captura de fotos ou vídeos.

Este exemplo usa um controle [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) para ajustar o nível de zoom atual. A seção a seguir mostra como ajustar o zoom com base em um gesto de pinçagem na tela.

[!code-xml[ZoomXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetZoomXAML)]

Confirme se o dispositivo de captura atual dá suporte ao **ZoomControl** verificando a propriedade [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn633819). Havendo suporte para o controle, você pode mostrar e habilitar a interface do usuário para esse recurso.

O valor do nível de zoom deve estar dentro do intervalo aceito pelo dispositivo e deve ser um incremento do tamanho da etapa com suporte. Obtenha os valores aceitos para o dispositivo atual verificando as propriedades [**Min**](https://msdn.microsoft.com/library/windows/apps/dn633817), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn608150) e [**Step**](https://msdn.microsoft.com/library/windows/apps/dn633818), que são usadas para definir as propriedades correspondentes do controle deslizante.

Defina o valor do controle deslizante como o valor atual de **ZoomControl** depois de cancelar o registro do manipulador de eventos [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) para que o evento não seja disparado quando o valor for definido.

[!code-cs[ZoomControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetZoomControl)]

No manipulador de eventos **ValueChanged**, crie uma nova instância da classe [**ZoomSettings**](https://msdn.microsoft.com/library/windows/apps/dn926722) definindo a propriedade [**Value**](https://msdn.microsoft.com/library/windows/apps/dn926724) como o valor atual do controle deslizante de zoom. Se a propriedade [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926721) do **ZoomControl** contiver [**ZoomTransitionMode.Smooth**](https://msdn.microsoft.com/library/windows/apps/dn926726), significa que o dispositivo permite transições suaves entre níveis de zoom. Como esse modo proporciona uma melhor experiência de usuário, normalmente é recomendável usar esse valor para a propriedade [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn926723) do objeto **ZoomSettings**.

Por fim, altere as configurações de zoom atuais transmitindo seu objeto **ZoomSettings** ao método [**Configure**](https://msdn.microsoft.com/library/windows/apps/dn926719) do objeto **ZoomControl**.

[!code-cs[ZoomSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetZoomSlider)]

### <a name="smooth-zoom-using-pinch-gesture"></a>Zoom suave usando o gesto de pinçagem

Conforme explicado na versão anterior, em dispositivos com suporte, o modo de zoom suave permite que o dispositivo de captura faça a transição suave entre níveis de zoom digitais, deixando que o usuário ajuste dinamicamente o nível de zoom durante a operação de captura sem transições discretas e chocantes. Esta seção descreve como ajustar o nível de zoom em resposta a um gesto de pinçagem.

Primeiro, determine se o controle de zoom digital tem suporte no dispositivo atual verificando a propriedade [**ZoomControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn633819). Em seguida, determine se o modo de zoom suave está disponível, verificando [**ZoomControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926721) para ver se este contém o valor [**ZoomTransitionMode.Smooth**](https://msdn.microsoft.com/library/windows/apps/dn926726).

[!code-cs[IsSmoothZoomSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsSmoothZoomSupported)]

Em um dispositivo habilitado para multitoque, um cenário típico é ajustar o fator de zoom com base em um gesto de pinçagem com dois dedos. Defina a propriedade [**ManipulationMode**](https://msdn.microsoft.com/library/windows/apps/br208948) do controle [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) como [**ManipulationModes.Scale**](https://msdn.microsoft.com/library/windows/apps/br227934) para habilitar o gesto de pinçagem. Em seguida, registre-se para o evento [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) que é acionado quando o gesto de pinçagem muda de tamanho.

[!code-cs[RegisterPinchGestureHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterPinchGestureHandler)]

No manipulador para o evento **ManipulationDelta**, atualize o fator de zoom com base na alteração do gesto de pinçagem do usuário. O valor [**ManipulationDelta.Scale**](https://msdn.microsoft.com/library/windows/apps/br242016) representa a mudança de escala do gesto de pinçagem, de forma que um pequeno aumento no tamanho da pinçagem seja um número um pouco maior que 1,0 e uma pequena queda no tamanho seja um número um pouco menor que 1,0. Neste exemplo, o valor atual do controle de zoom é multiplicado pelo delta da escala.

Antes de configurar o fator de zoom, certifique-se de que o valor não seja menor que o valor mínimo compatível com o dispositivo, conforme indicado pela propriedade [**ZoomControl.Min**](https://msdn.microsoft.com/library/windows/apps/dn633817). Além disso, certifique-se de que o valor seja menor que ou igual ao valor [**ZoomControl.Max**](https://msdn.microsoft.com/library/windows/apps/dn608150). Por fim, você deve garantir que o fator de zoom é um múltiplo do tamanho da etapa de zoom compatível com o dispositivo, conforme indicado pela propriedade [**etapa**](https://msdn.microsoft.com/library/windows/apps/dn633818) . Se o fator de zoom não atender a esses requisitos, uma exceção será lançada quando você tentar definir o nível de zoom no dispositivo de captura.

Defina o nível de zoom no dispositivo de captura criando um novo objeto [**ZoomSettings**](https://msdn.microsoft.com/library/windows/apps/dn926722). Defina a propriedade [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn926723) como [**ZoomTransitionMode.Smooth**](https://msdn.microsoft.com/library/windows/apps/dn926726) e a propriedade [**Value**](https://msdn.microsoft.com/library/windows/apps/dn926724) como o fator de zoom desejado. Por fim, chame [**ZoomControl.Configure**](https://msdn.microsoft.com/library/windows/apps/dn926719) para definir o novo valor de zoom no dispositivo. O dispositivo fará a transição suavemente para o novo valor de zoom.

[!code-cs[ManipulationDelta](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Captura básica de fotos, áudio e vídeo com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
