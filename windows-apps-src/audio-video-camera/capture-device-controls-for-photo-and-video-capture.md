---
ms.assetid: 831123A7-1F40-4B74-AE9F-69AC9883B4AD
description: Este artigo mostra como usar controles de dispositivo manuais para habilitar cenários de captura de fotos e vídeos avançados, incluindo estabilização de imagem óptica e zoom suave.
title: Controles manuais da câmera para a captura de fotos e vídeos
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 99ffa0dcae3412d49aef9da5bc3dfea255173ecb
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358943"
---
# <a name="manual-camera-controls-for-photo-and-video-capture"></a>Controles manuais da câmera para a captura de fotos e vídeos



Este artigo mostra como usar controles de dispositivo manuais para habilitar cenários de captura de fotos e vídeos avançados, incluindo estabilização de imagem óptica e zoom suave.

Os controles discutidos neste artigo são todos adicionados ao seu aplicativo usando o mesmo padrão. Primeiro, verifique se o controle tem suporte no dispositivo atual em que seu aplicativo está sendo executado. Se o controle tiver suporte, defina o modo desejado para o controle. Normalmente, se um determinado controle não tiver suporte no dispositivo atual, você deverá desabilitar ou ocultar o elemento da interface do usuário que permite ao usuário habilitar o recurso.

O código neste artigo foi adaptado da [amostra do SDK de Controles Manuais de Câmera](https://go.microsoft.com/fwlink/?linkid=845228). Você pode baixar o exemplo para ver o código usado no contexto ou utilizá-lo como ponto de partida para seu próprio app.

> [!NOTE]
> Este artigo se baseia em conceitos e códigos discutidos em [Captura básica de fotos, áudio e vídeo com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), que descreve as etapas para implementar uma captura básica de fotos e vídeos. Recomendamos que você se familiarize com o padrão de captura de mídia básica neste artigo antes de passar para cenários de captura mais avançados. O código deste artigo presume que seu aplicativo já tenha uma instância do MediaCapture inicializada corretamente.

Todas as APIs de controle de dispositivo discutidas neste artigo são membros do namespace [**Windows.Media.Devices**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices).

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

## <a name="exposure"></a>Exposição

O [**ExposureControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.ExposureControl) permite definir a velocidade do obturador usada durante a captura de foto ou vídeo.

Este exemplo usa um controle [**Slider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) para ajustar o valor de exposição atual e uma caixa de seleção para alternar o ajuste automático da exposição.

[!code-xml[ExposureXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetExposureXAML)]

Confirme se o dispositivo de captura atual dá suporte ao **ExposureControl** verificando a propriedade [**Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.exposurecontrol.supported). Havendo suporte para o controle, você pode mostrar e habilitar a interface do usuário para esse recurso. Defina o estado de marcação da caixa de seleção para indicar se o ajuste automático da exposição está atualmente ativo para o valor da propriedade [**Auto**](https://docs.microsoft.com/uwp/api/windows.media.devices.exposurecontrol.auto).

O valor da exposição deve estar dentro do intervalo aceito pelo dispositivo e deve ser um incremento do tamanho da etapa com suporte. Obtenha os valores aceitos para o dispositivo atual verificando as propriedades [**Min**](https://docs.microsoft.com/uwp/api/windows.media.devices.exposurecontrol.min), [**Max**](https://docs.microsoft.com/uwp/api/windows.media.devices.exposurecontrol.max) e [**Step**](https://docs.microsoft.com/uwp/api/windows.media.devices.exposurecontrol.step), que são usadas para definir as propriedades correspondentes do controle deslizante.

Defina o valor do controle deslizante como o valor atual de **ExposureControl** depois de cancelar o registro do manipulador de eventos [**ValueChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) para que o evento não seja disparado quando o valor for definido.

[!code-cs[ExposureControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureControl)]

No manipulador de eventos **ValueChanged**, obtenha o valor atual do controle e defina o valor da exposição chamando [**SetValueAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.exposurecontrol.setvalueasync).

[!code-cs[ExposureSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureSlider)]

No manipulador de eventos **CheckedChanged** da caixa de seleção de exposição automática, ative ou desative o ajuste automático da exposição chamando [**SetAutoAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.exposurecontrol.setautoasync) e transmitindo um valor booliano.

[!code-cs[ExposureCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureCheckBox)]

> [!IMPORTANT]
> Somente há suporte para o modo de exposição automática enquanto o fluxo de visualização estiver em execução. Verifique se o fluxo de visualização está em execução antes de ativar a exposição automática.

## <a name="exposure-compensation"></a>Compensação de exposição

O [**ExposureCompensationControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.ExposureCompensationControl) permite definir a compensação de exposição usada durante a captura de fotos ou vídeos.

Este exemplo usa um controle [**Slider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) para ajustar o valor atual da compensação de exposição.

[!code-xml[EvXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetEvXAML)]

Confirme se o dispositivo de captura atual dá suporte ao **ExposureCompensationControl** verificando a propriedade [Supported](supported-codecs.md). Havendo suporte para o controle, você pode mostrar e habilitar a interface do usuário para esse recurso.

O valor da compensação de exposição deve estar dentro do intervalo aceito pelo dispositivo e deve ser um incremento do tamanho da etapa com suporte. Obtenha os valores aceitos para o dispositivo atual verificando as propriedades [**Min**](https://docs.microsoft.com/uwp/api/windows.media.devices.exposurecompensationcontrol.min), [**Max**](https://docs.microsoft.com/uwp/api/windows.media.devices.exposurecompensationcontrol.max) e [**Step**](https://docs.microsoft.com/uwp/api/windows.media.devices.exposurecompensationcontrol.step), que são usadas para definir as propriedades correspondentes do controle deslizante.

Defina o valor do controle deslizante como o valor atual de **ExposureCompensationControl** depois de cancelar o registro do manipulador de eventos [**ValueChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) para que o evento não seja disparado quando o valor for definido.

[!code-cs[EvControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEvControl)]

No manipulador de eventos **ValueChanged**, obtenha o valor atual do controle e defina o valor da exposição chamando [**SetValueAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.exposurecompensationcontrol.setvalueasync).

[!code-cs[EvValueChanged](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEvValueChanged)]

## <a name="flash"></a>Flash

O [**FlashControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.FlashControl) permite que você habilite ou desabilite o flash ou habilite o flash automático, com o qual o sistema determina dinamicamente se deve ou não usar o flash. Esse controle também permite que você habilite a redução automática de olhos vermelhos em dispositivos com suporte para esse recurso. Todas essas configurações se aplicam à captura de fotos. O [**TorchControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.TorchControl) é um controle separado para ativar ou desativar a lanterna para captura de vídeos.

Este exemplo usa um conjunto de botões de opção para permitir que o usuário alterne entre as configurações de flash ativado, desativado e automático. Uma caixa de seleção também é fornecida para permitir a alternância entre a redução de olhos vermelhos e a lanterna de vídeo.

[!code-xml[FlashXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetFlashXAML)]

Verifique se o dispositivo de captura atual dá suporte ao **FlashControl** verificando a propriedade [**Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.supported). Havendo suporte para o controle, você pode mostrar e habilitar a interface do usuário para esse recurso. Se houver suporte para **FlashControl**, a redução automática de olhos vermelhos poderá ou não ser compatível. Portanto, verifique a propriedade [**RedEyeReductionSupported**](https://docs.microsoft.com/uwp/api/windows.media.devices.flashcontrol.redeyereductionsupported) antes de habilitar a interface do usuário. Como **TorchControl** é separado do controle de flash, você também deve verificar sua propriedade [**Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.torchcontrol.supported) antes de usá-lo.

No manipulador de eventos [**Checked**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked) para cada um dos botões de opção de flash, habilite ou desabilite a configuração de flash correspondente apropriada. Observe que, para definir o flash de forma que ele sempre seja usado, você deve definir a propriedade [**Enabled**](https://docs.microsoft.com/uwp/api/windows.media.devices.flashcontrol.enabled) como true e a propriedade [**Auto**](https://docs.microsoft.com/uwp/api/windows.media.devices.flashcontrol.auto) como false.

[!code-cs[FlashControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFlashControl)]

[!code-cs[FlashRadioButtons](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFlashRadioButtons)]

No manipulador da caixa de seleção de redução de olhos vermelhos, defina a propriedade [**RedEyeReduction**](https://docs.microsoft.com/uwp/api/windows.media.devices.flashcontrol.redeyereduction) como o valor apropriado.

[!code-cs[RedEye](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetRedEye)]

Por fim, no manipulador da caixa de seleção da lanterna de vídeo, defina a propriedade [**Enabled**](https://docs.microsoft.com/uwp/api/windows.media.devices.torchcontrol.enabled) como o valor apropriado.

[!code-cs[Torch](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTorch)]

> [!NOTE] 
>  Em alguns dispositivos, o lanterna não emitirá luz, mesmo se [**TorchControl.Enabled**](https://docs.microsoft.com/uwp/api/windows.media.devices.torchcontrol.enabled) for definido como true, a menos que o dispositivo tenha um fluxo de visualização em execução e esteja ativamente capturando vídeo. A ordem recomendada das operações é ativar a visualização de vídeo, ativar a lanterna definindo **Enabled** como true e depois iniciar a captura de vídeo. Em alguns dispositivos, a lanterna acende depois que a visualização é iniciada. Em outros dispositivos, o lanterna não pode acender até que captura de vídeo seja iniciada.

## <a name="focus"></a>Foco

Três métodos diferentes comumente usados para ajustar o foco da câmera são aceitos pelo objeto [**FocusControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.FocusControl): foco automático contínuo, tocar para focalizar e foco manual. Um aplicativo de câmera pode oferecer suporte a todos esses três métodos, mas para facilitar a leitura, este artigo descreve cada técnica separadamente. Esta seção também explica como habilitar a luz auxiliar de foco.

### <a name="continuous-autofocus"></a>Foco automático contínuo

Habilitar o foco automático contínuo instrui a câmera para ajustar o foco dinamicamente para tentar manter o sujeito da foto ou vídeo em foco. Este exemplo usa um botão de opção para ativar e desativar o foco automático contínuo.

[!code-xml[CAFXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetCAFXAML)]

Confirme se o dispositivo de captura atual dá suporte ao **FocusControl** verificando a propriedade [**Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.flashcontrol.supported). Em seguida, veja se há suporte para o foco automático verificando a lista [**SupportedFocusModes**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.supportedfocusmodes) para ver se ela contém o valor [**FocusMode.Continuous**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.FocusMode) e, se contiver, mostrar o botão de opção de foco automático contínuo.

[!code-cs[CAF](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetCAF)]

No manipulador de eventos [**Checked**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked) do botão de opção de foco automático contínuo, use a propriedade [**VideoDeviceController.FocusControl**](https://docs.microsoft.com/uwp/api/windows.media.devices.videodevicecontroller.focuscontrol) para obter uma instância do controle. Chame [**UnlockAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.unlockasync) para desbloquear o controle caso seu aplicativo tenha chamado [**LockAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.lockasync) anteriormente para habilitar um dos modos de foco.

Crie um novo objeto [**FocusSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.FocusSettings) e defina a propriedade [**Mode**](https://docs.microsoft.com/uwp/api/windows.media.devices.focussettings.mode) como **Continuous**. Defina a propriedade [**AutoFocusRange**](https://docs.microsoft.com/uwp/api/windows.media.devices.focussettings.autofocusrange) como um valor apropriado para o seu cenário de aplicativo ou como um valor selecionado pelo usuário na sua interface do usuário. Transmita o objeto **FocusSettings** para o método [**Configure**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.configure) e depois chame [**FocusAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.focusasync) para iniciar um foco automático contínuo.

[!code-cs[CafFocusRadioButton](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetCafFocusRadioButton)]

> [!IMPORTANT]
> O modo de foco automático é aceito somente enquanto o fluxo de visualização está em execução. Verifique se o fluxo de visualização está em execução antes de ativar o foco automático contínuo.

### <a name="tap-to-focus"></a>Tocar para focalizar

A técnica de tocar para focalizar usa o [**FocusControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.FocusControl) e o [**RegionsOfInterestControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.RegionsOfInterestControl) para especificar uma sub-região do quadro de captura na qual o dispositivo de captura deve focalizar. A região de foco é determinada pelo usuário ao tocar na tela que exibe o fluxo de visualização.

Este exemplo usa um botão de opção para habilitar e desabilitar o modo "tocar para focalizar".

[!code-xml[TapFocusXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetTapFocusXAML)]

Confirme se o dispositivo de captura atual dá suporte ao **FocusControl** verificando a propriedade [**Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.flashcontrol.supported). O **RegionsOfInterestControl** deve ter suporte e deve dar suporte a pelo menos uma região para usar essa técnica. Verifique as propriedades [**AutoFocusSupported**](https://docs.microsoft.com/uwp/api/windows.media.devices.regionsofinterestcontrol.autofocussupported) e [**MaxRegions**](https://docs.microsoft.com/uwp/api/windows.media.devices.regionsofinterestcontrol.maxregions) para determinar se o botão de opção de tocar para focalizar deve ser mostrado ou ocultado.

[!code-cs[TapFocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocus)]

No manipulador de eventos [**Checked**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked) do botão de opção de tocar para focalizar, use a propriedade [**VideoDeviceController.FocusControl**](https://docs.microsoft.com/uwp/api/windows.media.devices.videodevicecontroller.focuscontrol) para obter uma instância do controle. Chame [**LockAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.lockasync) para bloquear o controle caso seu aplicativo tenha sido chamado anteriormente [**UnlockAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.unlockasync) para habilitar o foco automático contínuo e aguarde até que o usuário toque na tela para alterar o foco.

[!code-cs[TapFocusRadioButton](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocusRadioButton)]

Este exemplo foca em uma região quando o usuário toca na tela e, em seguida, remove o foco dessa região quando o usuário toca novamente, como uma alternância. Use uma variável booliana para rastrear o estado de alternância atual.

[!code-cs[IsFocused](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsFocused)]

A próxima etapa é escutar o evento quando o usuário tocar na tela, manipulando o evento [**Tapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped) do [**CaptureElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) que está exibindo atualmente o fluxo de visualização da captura. Se a câmera não estiver atualmente em visualização, ou se o modo de toque para foco estiver desabilitado, retorne a partir do manipulador sem fazer nada.

Se a variável de controle  *\_isFocused* seja alternado para false, e se a câmera não estiver atualmente no processo de foco (determinado pelo [ **FocusState** ](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.focusstate) propriedade do **FocusControl**), iniciar o processo de foco de toque. Obtenha a posição de toque do usuário dos argumentos de evento passados para o manipulador. Este exemplo também aproveita a oportunidade para selecionar o tamanho da região que será focalizada. Neste caso, o tamanho é 1/4 da menor dimensão do elemento de captura. Passe a posição do toque e o tamanho da região para o método auxiliar **TapToFocus** que é definido na próxima seção.

Se o  *\_isFocused* alternância está definida como true, o usuário toque deve limpar o foco da região anterior. Isso é feito no método auxiliar **TapUnfocus**, mostrado abaixo.

[!code-cs[TapFocusPreviewControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocusPreviewControl)]

No **TapToFocus** método auxiliar, defina primeiro o  *\_isFocused* alternância como true para que o toque de tela próxima liberará o foco da região tocado.

A próxima tarefa nesse método auxiliar é determinar o retângulo dentro do fluxo de visualização será atribuído ao controle de foco. Isso exige duas etapas. A primeira etapa é determinar o retângulo que o fluxo de visualização ocupa dentro do controle [**CaptureElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement). Isso depende das dimensões do fluxo de visualização e da orientação do dispositivo. O método auxiliar **GetPreviewStreamRectInControl**, mostrado no final desta seção, executa essa tarefa e retorna o retângulo que contém o fluxo de visualização.

A próxima tarefa em **TapToFocus** é converter o local de toque e o tamanho do retângulo de foco desejado, que foram determinados no manipulador de eventos **CaptureElement.Tapped**, em coordenadas no fluxo de captura. O método auxiliar **ConvertUiTapToPreviewRect**, mostrado posteriormente nesta seção, executa essa conversão e retorna o retângulo, nas coordenadas do fluxo de captura, onde o foco será solicitado.

Agora que o retângulo de destino foi obtido, crie um novo objeto [**RegionOfInterest**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.RegionOfInterest), definindo a propriedade [**Bounds**](https://docs.microsoft.com/uwp/api/windows.media.devices.regionofinterest.bounds) como o retângulo de destino obtido nas etapas anteriores.

Obtenha o [**FocusControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.FocusControl) do dispositivo de captura. Criar um novo objeto [**FocusSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.FocusSettings) e defina [**Mode**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.mode) e [**AutoFocusRange**](https://docs.microsoft.com/uwp/api/windows.media.devices.focussettings.autofocusrange) como seus valores desejados depois de verificar se eles têm suporte por **FocusControl**. Chame [**Configure**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.configure) no **FocusControl** para ativar as configurações e sinalizar o dispositivo para começar a focalizar a região especificada.

Em seguida, obtenha o [**RegionsOfInterestControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.RegionsOfInterestControl) do dispositivo de captura e chame [**SetRegionsAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.regionsofinterestcontrol.setregionsasync) para definir a região ativa. Várias regiões de interesse podem ser definidas em dispositivos que permitem isso, mas este exemplo define apenas uma região.

Por fim, chame [**FocusAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.focusasync) no **FocusControl** para iniciar a focalização.

> [!IMPORTANT]
> Ao implementar tocar para focalizar, a ordem das operações é importante. Você deve chamar essas APIs na seguinte ordem:
>
> 1. [**FocusControl.Configure**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.configure)
> 2. [**RegionsOfInterestControl.SetRegionsAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.regionsofinterestcontrol.setregionsasync)
> 3. [**FocusControl.FocusAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.focusasync)

[!code-cs[TapToFocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapToFocus)]

No método auxiliar **TapUnfocus**, obtenha o **RegionsOfInterestControl** e chame [**ClearRegionsAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.regionsofinterestcontrol.clearregionsasync) para limpar a região que foi registrada com o controle no método auxiliar **TapToFocus**. Em seguida, obtenha o **FocusControl** e chame [**FocusAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.focusasync) para fazer com que o dispositivo focalize novamente sem uma região de interesse.

[!code-cs[TapUnfocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapUnfocus)]

O método auxiliar **GetPreviewStreamRectInControl** usa a resolução do fluxo de visualização e a orientação do dispositivo para determinar o retângulo dentro do elemento de visualização que contém o fluxo de visualização, cortando qualquer preenchimento em letterbox que o controle possa fornecer para manter a taxa de proporção do fluxo. Este método usa variáveis membros da classe definidas no código de exemplo de captura de mídia básica encontrado em [Captura básica de fotos, aúdio e vídeo com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

[!code-cs[GetPreviewStreamRectInControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetGetPreviewStreamRectInControl)]

O método auxiliar **ConvertUiTapToPreviewRect** assume como argumentos o local do evento de toque, o tamanho desejado da região de foco e o retângulo que contém o fluxo de visualização obtido do método auxiliar **GetPreviewStreamRectInControl**. Esse método usa esses valores e orientação atual do dispositivo para calcular o retângulo dentro do fluxo de visualização que contém a região desejada. Mais uma vez, esse método usa variáveis membros da classe definidas no código de exemplo de captura de mídia básica encontrado em [Capturar fotos e vídeos com o MediaCapture](capture-photos-and-video-with-mediacapture.md).

[!code-cs[ConvertUiTapToPreviewRect](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetConvertUiTapToPreviewRect)]

### <a name="manual-focus"></a>Foco manual

A técnica de foco manual usa um controle **Slider** para definir a intensidade de foco atual do dispositivo de captura. Um botão de opção é usado para ativar e desativar o foco manual.

[!code-xml[ManualFocusXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetManualFocusXAML)]

Confirme se o dispositivo de captura atual dá suporte ao **FocusControl** verificando a propriedade [**Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.supported). Havendo suporte para o controle, você pode mostrar e habilitar a interface do usuário para esse recurso.

O valor do foco deve estar dentro do intervalo aceito pelo dispositivo e deve ser um incremento do tamanho da etapa com suporte. Obtenha os valores aceitos para o dispositivo atual verificando as propriedades [**Min**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.min), [**Max**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.max) e [**Step**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.step), que são usadas para definir as propriedades correspondentes do controle deslizante.

Defina o valor do controle deslizante como o valor atual de **FocusControl** depois de cancelar o registro do manipulador de eventos [**ValueChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) para que o evento não seja disparado quando o valor for definido.

[!code-cs[Focus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocus)]

No manipulador de eventos **Checked** para o botão de opção de foco manual, obtenha o objeto **FocusControl** e chame [**LockAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.lockasync) caso seu aplicativo tenha desbloqueado anteriormente o foco com uma chamada para [**UnlockAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.unlockasync).

[!code-cs[ManualFocusChecked](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetManualFocusChecked)]

No manipulador de eventos **ValueChanged** do controle deslizante de foco manual, obtenha o valor atual do controle e defina o valor do foco chamando [**SetValueAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.focuscontrol.setvalueasync).

[!code-cs[FocusSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusSlider)]

### <a name="enable-the-focus-light"></a>Habilitar a luz de foco

Em dispositivos com suporte, você pode habilitar uma luz auxiliar de foco para ajudar no foco do dispositivo. Este exemplo usa uma caixa de seleção para habilitar ou desabilitar a luz auxiliar de foco.

[!code-xml[FocusLightXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetFocusLightXAML)]

Verifique se o dispositivo de captura atual dá suporte ao **FlashControl** verificando a propriedade [**Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.flashcontrol.supported). Também verifique o [**AssistantLightSupported**](https://docs.microsoft.com/uwp/api/windows.media.devices.flashcontrol.assistantlightsupported) para saber se também há suporte para a luz auxiliar. Havendo suporte para ambos, você pode mostrar e habilitar a interface do usuário para esse recurso.

[!code-cs[FocusLight](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusLight)]

No manipulador de eventos **CheckedChanged**, obtenha o objeto [**FlashControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.FlashControl) dos dispositivos de captura. Defina a propriedade [**AssistantLightEnabled**](https://docs.microsoft.com/uwp/api/windows.media.devices.flashcontrol.assistantlightenabled) para habilitar ou desabilitar a luz de foco.

[!code-cs[FocusLightCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusLightCheckBox)]

## <a name="iso-speed"></a>Velocidade ISO

O [**IsoSpeedControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.IsoSpeedControl) permite definir a velocidade ISO usada durante a captura de fotos ou vídeos.

Este exemplo usa um controle [**Slider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) para ajustar o valor de compensação da exposição atual e uma caixa de seleção para alternar o ajuste automático da velocidade ISO.

[!code-xml[IsoXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetIsoXAML)]

Confirme se o dispositivo de captura atual dá suporte ao **IsoSpeedControl** verificando a propriedade [**Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.isospeedcontrol.supported). Havendo suporte para o controle, você pode mostrar e habilitar a interface do usuário para esse recurso. Defina o estado de marcação da caixa de seleção para indicar se o ajuste automático da velocidade ISO está atualmente ativo para o valor da propriedade [**Auto**](https://docs.microsoft.com/uwp/api/windows.media.devices.isospeedcontrol.auto).

O valor da velocidade ISO deve estar dentro do intervalo aceito pelo dispositivo e deve ser um incremento do tamanho da etapa com suporte. Obtenha os valores aceitos para o dispositivo atual verificando as propriedades [**Min**](https://docs.microsoft.com/uwp/api/windows.media.devices.isospeedcontrol.min), [**Max**](https://docs.microsoft.com/uwp/api/windows.media.devices.isospeedcontrol.max) e [**Step**](https://docs.microsoft.com/uwp/api/windows.media.devices.isospeedcontrol.step), que são usadas para definir as propriedades correspondentes do controle deslizante.

Defina o valor do controle deslizante como o valor atual de **IsoSpeedControl** depois de cancelar o registro do manipulador de eventos [**ValueChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) para que o evento não seja disparado quando o valor for definido.

[!code-cs[IsoControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoControl)]

No manipulador de eventos **ValueChanged**, obtenha o valor atual do controle e defina o valor da velocidade ISO chamando [**SetValueAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.isospeedcontrol.setvalueasync).

[!code-cs[IsoSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoSlider)]

No manipulador de eventos **CheckedChanged** da caixa de seleção de velocidade ISO automática, ative o ajuste automático da velocidade ISO chamando [**SetAutoAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.isospeedcontrol.setautoasync). Desative o ajuste automático da velocidade ISO chamando [**SetValueAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.isospeedcontrol.setvalueasync) e transmitindo o valor atual do controle deslizante.

[!code-cs[IsoCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoCheckBox)]

## <a name="optical-image-stabilization"></a>Estabilização de imagem ótica

A OIS (estabilização de imagem ótica) estabiliza um fluxo de vídeo capturado manipulando mecanicamente o hardware do dispositivo de captura, o que pode fornecer um resultado superior ao da estabilização digital. Em dispositivos que não dão suporte à OIS, você pode usar o VideoStabilizationEffect para executar a estabilização digital nos vídeos capturados. Para obter mais informações, consulte [Efeitos para captura de vídeo](effects-for-video-capture.md).

Determine se a OIS tem suporte no dispositivo atual, verificando a propriedade [**OpticalImageStabilizationControl.Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.opticalimagestabilizationcontrol.supported).

O controle OIS dá suporte a três modos: ativado, desativado e automático, o que significa que o dispositivo determina dinamicamente se a OIS melhoraria a captura de mídia e, em caso afirmativo, habilita a OIS. Para determinar se um modo específico tem suporte em um dispositivo, verifique se a coleção [**OpticalImageStabilizationControl.SupportedModes**](https://docs.microsoft.com/uwp/api/windows.media.devices.opticalimagestabilizationcontrol.supportedmodes) contém o modo desejado.

Habilite ou desabilite a OIS definindo o [**OpticalImageStabilizationControl.Mode**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.OpticalImageStabilizationMode) como o modo desejado.

[!code-cs[SetOpticalImageStabilizationMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetOpticalImageStabilizationMode)]

## <a name="powerline-frequency"></a>Frequência de Powerline
Alguns dispositivos de câmera oferecem suporte ao processamento anticintilação que depende de saber a frequência de CA de powerlines no ambiente atual. Alguns dispositivos oferecem suporte à determinação automática da frequência de powerline, enquanto outras exigem que a frequência seja definida manualmente. O exemplo de código a seguir mostra como determinar o suporte à frequência de powerline no dispositivo e, se necessário, como definir a frequência manualmente. 

Primeiro, chame o método **VideoDeviceController**[**TryGetPowerlineFrequency**](https://docs.microsoft.com/uwp/api/windows.media.devices.videodevicecontroller.trygetpowerlinefrequency), passando um parâmetro de saída do tipo [**PowerlineFrequency**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.PowerlineFrequency); se essa chamada falhar, o controle de frequência de powerline não terá suporte no dispositivo atual. Se houver suporte para o recurso, você poderá determinar se o modo automático está disponível no dispositivo tentando definir o modo automático. Faça isso chamando [**TrySetPowerlineFrequency**](https://docs.microsoft.com/uwp/api/windows.media.devices.videodevicecontroller.trysetpowerlinefrequency) e passando o valor **Automático**. Se a chamada tiver êxito, significa que sua frequência de powerline automática é compatível. Se o controlador de frequência de powerline for compatível com o dispositivo, mas a detecção automática de frequência não for, você ainda poderá definir a frequência manualmente usando **TrySetPowerlineFrequency**. Neste exemplo, **MyCustomFrequencyLookup** é um método personalizado que você implementa para determinar a frequência correta para o local atual do dispositivo. 

[!code-cs[PowerlineFrequency](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetPowerlineFrequency)]

## <a name="white-balance"></a>Proporção de branco

O [**WhiteBalanceControl**](https://docs.microsoft.com/uwp/api/windows.media.devices.videodevicecontroller.whitebalancecontrol) permite definir a proporção de branco usada durante a captura de fotos ou vídeos.

Este exemplo usa um controle [**ComboBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) para selecionar uma das predefinições de temperatura de cor internas e um controle [**Slider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) para o ajuste manual da proporção de branco.

[!code-xml[WhiteBalanceXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetWhiteBalanceXAML)]

Confirme se o dispositivo de captura atual dá suporte ao **WhiteBalanceControl** verificando a propriedade [**Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.whitebalancecontrol.supported). Havendo suporte para o controle, você pode mostrar e habilitar a interface do usuário para esse recurso. Defina os itens da caixa de combinação com os valores da enumeração [**ColorTemperaturePreset**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.ColorTemperaturePreset). Defina também o item selecionado como o valor atual da propriedade [**Preset**](https://docs.microsoft.com/uwp/api/windows.media.devices.whitebalancecontrol.preset).

Para controle manual, o valor da proporção de branco deve estar dentro do intervalo permitido pelo dispositivo e deve ser um incremento do tamanho da etapa com suporte. Obtenha os valores aceitos para o dispositivo atual verificando as propriedades [**Min**](https://docs.microsoft.com/uwp/api/windows.media.devices.whitebalancecontrol.min), [**Max**](https://docs.microsoft.com/uwp/api/windows.media.devices.whitebalancecontrol.max) e [**Step**](https://docs.microsoft.com/uwp/api/windows.media.devices.whitebalancecontrol.step), que são usadas para definir as propriedades correspondentes do controle deslizante. Antes de habilitar o controle manual, certifique-se de que o intervalo entre os valores mínimos e máximo com suporte seja maior que o tamanho da etapa. Caso contrário, o controle manual não terá suporte no dispositivo atual.

Defina o valor do controle deslizante como o valor atual de **WhiteBalanceControl** depois de cancelar o registro do manipulador de eventos [**ValueChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) para que o evento não seja disparado quando o valor for definido.

[!code-cs[WhiteBalance](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalance)]

No manipulador de eventos [**SelectionChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) da caixa de combinação de predefinições de temperatura de cor, obtenha a predefinição atualmente selecionada e defina o valor do controle chamando [**SetPresetAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.whitebalancecontrol.setpresetasync). Se o valor predefinido selecionado não for **Manual**, desabilite o controle deslizante de proporção de branco manual.

[!code-cs[WhiteBalanceComboBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalanceComboBox)]

No manipulador de eventos **ValueChanged**, obtenha o valor atual do controle e defina o valor da proporção de branco chamando [**SetValueAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.exposurecontrol.setvalueasync).

[!code-cs[WhiteBalanceSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalanceSlider)]

> [!IMPORTANT]
> O ajuste da proporção de branco só é permitido enquanto o fluxo de visualização estiver em execução. Verifique se o fluxo de visualização está em execução antes de definir o valor ou a predefinição da proporção de branco.

> [!IMPORTANT]
> O valor predefinido **ColorTemperaturePreset.Auto** instrui o sistema a ajustar automaticamente o nível de proporção de branco. Para alguns cenários, como a captura de uma sequência de fotos em que os níveis de proporção de branco devem ser iguais para cada quadro, convém bloquear o controle no valor automático atual. Para fazer isso, chame [**SetPresetAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.whitebalancecontrol.setpresetasync) e especifique a predefinição **Manual** e não defina um valor no controle usando [**SetValueAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.whitebalancecontrol.setvalueasync). Isso fará com que o dispositivo bloqueie o valor atual. Não tente ler o valor atual do controle atual e depois transmitir o valor retornado para **SetValueAsync**, pois não há garantia de que esse valor seja correto.

## <a name="zoom"></a>Zoom

O [**ZoomControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.ZoomControl) permite definir o nível de zoom usado durante a captura de fotos ou vídeos.

Este exemplo usa um controle [**Slider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) para ajustar o nível de zoom atual. A seção a seguir mostra como ajustar o zoom com base em um gesto de pinçagem na tela.

[!code-xml[ZoomXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetZoomXAML)]

Confirme se o dispositivo de captura atual dá suporte ao **ZoomControl** verificando a propriedade [**Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.zoomcontrol.supported). Havendo suporte para o controle, você pode mostrar e habilitar a interface do usuário para esse recurso.

O valor do nível de zoom deve estar dentro do intervalo aceito pelo dispositivo e deve ser um incremento do tamanho da etapa com suporte. Obtenha os valores aceitos para o dispositivo atual verificando as propriedades [**Min**](https://docs.microsoft.com/uwp/api/windows.media.devices.zoomcontrol.min), [**Max**](https://docs.microsoft.com/uwp/api/windows.media.devices.zoomcontrol.max) e [**Step**](https://docs.microsoft.com/uwp/api/windows.media.devices.zoomcontrol.step), que são usadas para definir as propriedades correspondentes do controle deslizante.

Defina o valor do controle deslizante como o valor atual de **ZoomControl** depois de cancelar o registro do manipulador de eventos [**ValueChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) para que o evento não seja disparado quando o valor for definido.

[!code-cs[ZoomControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetZoomControl)]

No manipulador de eventos **ValueChanged**, crie uma nova instância da classe [**ZoomSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.ZoomSettings) definindo a propriedade [**Value**](https://docs.microsoft.com/uwp/api/windows.media.devices.zoomsettings.value) como o valor atual do controle deslizante de zoom. Se a propriedade [**SupportedModes**](https://docs.microsoft.com/uwp/api/windows.media.devices.zoomcontrol.supportedmodes) do **ZoomControl** contiver [**ZoomTransitionMode.Smooth**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.ZoomTransitionMode), significa que o dispositivo permite transições suaves entre níveis de zoom. Como esse modo proporciona uma melhor experiência de usuário, normalmente é recomendável usar esse valor para a propriedade [**Mode**](https://docs.microsoft.com/uwp/api/windows.media.devices.zoomsettings.mode) do objeto **ZoomSettings**.

Por fim, altere as configurações de zoom atuais transmitindo seu objeto **ZoomSettings** ao método [**Configure**](https://docs.microsoft.com/uwp/api/windows.media.devices.zoomcontrol.configure) do objeto **ZoomControl**.

[!code-cs[ZoomSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetZoomSlider)]

### <a name="smooth-zoom-using-pinch-gesture"></a>Zoom suave usando o gesto de pinçagem

Conforme explicado na versão anterior, em dispositivos com suporte, o modo de zoom suave permite que o dispositivo de captura faça a transição suave entre níveis de zoom digitais, deixando que o usuário ajuste dinamicamente o nível de zoom durante a operação de captura sem transições discretas e chocantes. Esta seção descreve como ajustar o nível de zoom em resposta a um gesto de pinçagem.

Primeiro, determine se o controle de zoom digital tem suporte no dispositivo atual verificando a propriedade [**ZoomControl.Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.zoomcontrol.supported). Em seguida, determine se o modo de zoom suave está disponível, verificando [**ZoomControl.SupportedModes**](https://docs.microsoft.com/uwp/api/windows.media.devices.zoomcontrol.supportedmodes) para ver se este contém o valor [**ZoomTransitionMode.Smooth**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.ZoomTransitionMode).

[!code-cs[IsSmoothZoomSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsSmoothZoomSupported)]

Em um dispositivo habilitado para multitoque, um cenário típico é ajustar o fator de zoom com base em um gesto de pinçagem com dois dedos. Defina a propriedade [**ManipulationMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationmode) do controle [**CaptureElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) como [**ManipulationModes.Scale**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ManipulationModes) para habilitar o gesto de pinçagem. Em seguida, registre-se para o evento [**ManipulationDelta**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationdelta) que é acionado quando o gesto de pinçagem muda de tamanho.

[!code-cs[RegisterPinchGestureHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterPinchGestureHandler)]

No manipulador para o evento **ManipulationDelta**, atualize o fator de zoom com base na alteração do gesto de pinçagem do usuário. O valor [**ManipulationDelta.Scale**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.ManipulationDelta) representa a mudança de escala do gesto de pinçagem, de forma que um pequeno aumento no tamanho da pinçagem seja um número um pouco maior que 1,0 e uma pequena queda no tamanho seja um número um pouco menor que 1,0. Neste exemplo, o valor atual do controle de zoom é multiplicado pelo delta da escala.

Antes de configurar o fator de zoom, certifique-se de que o valor não seja menor que o valor mínimo compatível com o dispositivo, conforme indicado pela propriedade [**ZoomControl.Min**](https://docs.microsoft.com/uwp/api/windows.media.devices.zoomcontrol.min). Além disso, certifique-se de que o valor seja menor que ou igual ao valor [**ZoomControl.Max**](https://docs.microsoft.com/uwp/api/windows.media.devices.zoomcontrol.max). Por fim, certifique-se de que o fator de zoom é um múltiplo do tamanho de etapa do zoom com suporte pelo dispositivo, conforme indicado pelo [ **etapa** ](https://docs.microsoft.com/uwp/api/windows.media.devices.zoomcontrol.step) propriedade. Se o fator de zoom não atender a esses requisitos, uma exceção será lançada quando você tentar definir o nível de zoom no dispositivo de captura.

Defina o nível de zoom no dispositivo de captura criando um novo objeto [**ZoomSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.ZoomSettings). Defina a propriedade [**Mode**](https://docs.microsoft.com/uwp/api/windows.media.devices.zoomsettings.mode) como [**ZoomTransitionMode.Smooth**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.ZoomTransitionMode) e a propriedade [**Value**](https://docs.microsoft.com/uwp/api/windows.media.devices.zoomsettings.value) como o fator de zoom desejado. Por fim, chame [**ZoomControl.Configure**](https://docs.microsoft.com/uwp/api/windows.media.devices.zoomcontrol.configure) para definir o novo valor de zoom no dispositivo. O dispositivo fará a transição suavemente para o novo valor de zoom.

[!code-cs[ManipulationDelta](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Básica de fotos, vídeo e áudio capturar com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
