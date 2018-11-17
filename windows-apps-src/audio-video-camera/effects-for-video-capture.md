---
author: drewbatgit
ms.assetid: E0189423-1DF3-4052-AB2E-846EA18254C4
description: Este tópico mostra como aplicar efeitos à visualização de câmera e fluxos de gravação de vídeo e mostra como usar o efeito de estabilização de vídeo.
title: Efeitos para captura de vídeo
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d427a532e9821b81b6f23d08babecd692c8c95e1
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7104294"
---
# <a name="effects-for-video-capture"></a>Efeitos para captura de vídeo


Este tópico mostra como aplicar efeitos à visualização de câmera e fluxos de gravação de vídeo e mostra como usar o efeito de estabilização de vídeo.

> [!NOTE] 
> Este artigo se baseia em conceitos e códigos discutidos em [Captura básica de fotos, áudio e vídeo com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), que descreve as etapas para implementar uma captura básica de fotos e vídeos. Recomendamos que você se familiarize com o padrão de captura de mídia básica neste artigo antes de passar para cenários de captura mais avançados. O código deste artigo presume que seu aplicativo já tenha uma instância do MediaCapture inicializada corretamente.

## <a name="adding-and-removing-effects-from-the-camera-video-stream"></a>Adicionando e removendo efeitos do fluxo de vídeo da câmera
Para capturar ou visualizar vídeos da câmera do dispositivo, use o objeto [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) conforme descrito em [Captura básica de fotos, áudio e vídeo com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md). Após ter inicializado o objeto **MediaCapture**, você pode adicionar um ou mais efeitos de vídeo ao fluxo de visualização ou captura chamando [**AddVideoEffectAsync**](https://msdn.microsoft.com/library/windows/apps/dn878035), transmitindo um objeto [**IVideoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IVideoEffectDefinition) que representa o efeito a ser adicionado e um membro da enumeração [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaStreamType) que indica se o efeito deve ser adicionado ao fluxo de visualização da câmera ou ao fluxo de registro.

> [!NOTE]
> Em alguns dispositivos, o fluxo de visualização e o fluxo de captura são os mesmos, o que significa que, se você especificar **MediaStreamType.VideoPreview** ou **MediaStreamType.VideoRecord** ao chamar **AddVideoEffectAsync**, o efeito será aplicado aos fluxos de visualização e gravação. Você pode determinar se os fluxos de visualização e gravação são os mesmos no dispositivo atual verificando a propriedade [**VideoDeviceCharacteristic**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSettings.VideoDeviceCharacteristic) do objeto [**MediaCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.MediaCaptureSettings) para o **MediaCapture**. Se o valor dessa propriedade for **VideoDeviceCharacteristic.AllStreamsIdentical** ou **VideoDeviceCharacteristic.PreviewRecordStreamsIdentical**, os fluxos são os mesmos e qualquer efeito que se aplica a um afeta o outro.

O exemplo a seguir adiciona um efeito aos fluxos de visualização e gravação da câmera. Esse exemplo ilustra a verificação de se os fluxos de visualização e gravação são os mesmos.

[!code-cs[BasicAddEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetBasicAddEffect)]

Observe que **AddVideoEffectAsync** retorna um objeto que implementa [**IMediaExtension**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.IMediaExtension) que representa o efeito de vídeo adicionado. Alguns efeitos permitem que você altere as configurações de efeito transmitindo um [**PropertySet**](https://msdn.microsoft.com/library/windows/apps/Windows.Foundation.Collections.PropertySet) para o método [**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986).

A partir do Windows 10, versão 1607, você também pode usar o objeto retornado por **AddVideoEffectAsync** para remover o efeito do pipeline do vídeo transmitindo-o para [**RemoveEffectAsync**](https://msdn.microsoft.com/library/windows/apps/mt667957). **RemoveEffectAsync** determina automaticamente se o parâmetro de objeto de efeito foi adicionado ao fluxo de visualização ou gravação. Portanto, você não precisa especificar o tipo de fluxo ao fazer a chamada.

[!code-cs[RemoveOneEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetRemoveOneEffect)]

Você também pode remover todos os efeitos do fluxo de visualização ou captura chamando [**ClearEffectsAsync**](https://msdn.microsoft.com/library/windows/apps/br226592) e especificando o fluxo para o qual todos os efeitos devem ser removidos.

[!code-cs[ClearAllEffects](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetClearAllEffects)]

## <a name="video-stabilization-effect"></a>Efeito de estabilização de vídeo

O efeito de estabilização de vídeo manipula os quadros de um fluxo de vídeo para minimizar a vibração causada por segurar o dispositivo de captura em sua mão. Como essa técnica faz com que os pixels sejam deslocados para a direita, para a esquerda, para cima e para baixo, e como o efeito identifica o conteúdo fora do quadro de vídeo, o vídeo estabilizado é cortado ligeiramente do vídeo original. Uma função utilitária é fornecida para permitir que você ajuste suas configurações de codificação para gerenciar de forma ideal o corte realizado pelo efeito.

Nos dispositivos com suporte para isso, a OIS (estabilização de imagem ótica) estabiliza o vídeo manipulando mecanicamente o dispositivo de captura e, portanto, não precisa cortar as bordas dos quadros do vídeo. Para obter mais informações, consulte [Controles de captura do dispositivo para a captura de vídeo](capture-device-controls-for-video-capture.md).

### <a name="set-up-your-app-to-use-video-stabilization"></a>Configurar seu aplicativo para usar estabilização de vídeo

Além dos namespaces necessários para captura de mídia básica, o uso do efeito de estabilização de vídeo exige o namespace a seguir.

[!code-cs[VideoStabilizationEffectUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetVideoStabilizationEffectUsing)]

Declare uma variável membro para armazenar o objeto [**VideoStabilizationEffect**](https://msdn.microsoft.com/library/windows/apps/dn926760). Como parte da implementação do efeito, você modificará as propriedades de codificação que usa para codificar o vídeo capturado. Declare duas variáveis para armazenar uma cópia de backup das propriedades de codificação de entrada e saída para poder restaurá-las mais tarde, quando o efeito estiver desabilitado. Por fim, declare uma variável membro do tipo [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026), pois esse objeto será acessado em vários locais dentro do seu código.

[!code-cs[DeclareVideoStabilizationEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetDeclareVideoStabilizationEffect)]

Para esse cenário, você deve atribuir o objeto de perfil de codificação de mídia a uma variável membro para poder acessá-lo posteriormente.

[!code-cs[EncodingProfileMember](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetEncodingProfileMember)]

### <a name="initialize-the-video-stabilization-effect"></a>Inicializar o efeito de estabilização de vídeo

Após a inicialização do seu objeto **MediaCapture**, crie uma nova instância do objeto [**VideoStabilizationEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn926762). Chame [**MediaCapture.AddVideoEffectAsync**](https://msdn.microsoft.com/library/windows/apps/dn878035) para adicionar o efeito ao pipeline de vídeo e recuperar uma instância da classe [**VideoStabilizationEffect**](https://msdn.microsoft.com/library/windows/apps/dn926760). Especifique [**MediaStreamType.VideoRecord**](https://msdn.microsoft.com/library/windows/apps/br226640) para indicar que o efeito deve ser aplicado ao fluxo de gravação de vídeo.

Registre um manipulador de eventos para o evento [**EnabledChanged**](https://msdn.microsoft.com/library/windows/apps/dn948982) e chame o método auxiliar **SetUpVideoStabilizationRecommendationAsync**, ambos discutidos mais adiante neste artigo. Por fim, defina a propriedade [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926775) do efeito como "true" para habilitá-lo.

[!code-cs[CreateVideoStabilizationEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetCreateVideoStabilizationEffect)]

### <a name="use-recommended-encoding-properties"></a>Usar as propriedades de codificação recomendadas

Conforme discutido anteriormente neste artigo, a técnica usada pelo efeito de estabilização de vídeo faz inevitavelmente com que o vídeo estabilizado seja cortado em uma certa medida da fonte de vídeo. Defina a função de auxiliar a seguir em seu código para ajustar as propriedades de codificação de vídeo para lidar do modo ideal com essa limitação do efeito. Esta etapa não é necessária para usar o efeito de estabilização de vídeo, mas, se você não executá-la, o vídeo resultante será ampliado um pouco e, portanto, terá uma fidelidade visual um pouco menor.

Chame [**GetRecommendedStreamConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn948983) na sua instância de efeito de estabilização de vídeo, transmitindo o objeto [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825), que informa o efeito sobre as suas propriedades de codificação de fluxo de entrada atuais, e seu **MediaEncodingProfile**, que faz com que o evento conheça as suas propriedades de codificação de saída atuais. Esse método retorna um objeto [**VideoStreamConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn926727) contendo novas propriedades de codificação de fluxo de entrada e saída recomendadas.

As propriedades recomendadas de codificação de entrada apresentam, caso o dispositivo ofereça suporte, uma resolução mais alta do que as configurações iniciais que você forneceu, para que haja o mínimo de perda na resolução após a aplicação do efeito de corte.

Chame [**VideoDeviceController.SetMediaStreamPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh700895) para definir as novas propriedades de codificação. Antes de definir as novas propriedades, use a variável membro para armazenar as propriedades de codificação iniciais para que você possa modificar as configurações de novo quando desabilitar o efeito.

Se o efeito de estabilização de vídeo precisar cortar o vídeo de saída, as propriedades de codificação de saída recomendadas serão do tamanho do vídeo cortado. Isso significa que a resolução de saída corresponderá ao tamanho do vídeo cortado. Se você não usar as propriedades de saída recomendadas, o vídeo será dimensionado para corresponder ao tamanho de saída inicial, o que resultará em uma perda de fidelidade visual.

Defina a propriedade [**Video**](https://msdn.microsoft.com/library/windows/apps/hh701124) do objeto **MediaEncodingProfile**. Antes de definir as novas propriedades, use a variável membro para armazenar as propriedades de codificação iniciais para que você possa modificar as configurações de novo quando desabilitar o efeito.

[!code-cs[SetUpVideoStabilizationRecommendationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetSetUpVideoStabilizationRecommendationAsync)]

### <a name="handle-the-video-stabilization-effect-being-disabled"></a>Manipular o efeito de estabilização de vídeo sendo desabilitado

O sistema poderá desabilitar automaticamente o efeito de estabilização de vídeo se a taxa de transferência de pixel for muito alta para o efeito ou se detectar que o efeito está sendo executado lentamente. Se isso ocorrer, o evento EnabledChanged será gerado. A instância **VideoStabilizationEffect** no parâmetro *sender* indica o novo estado do efeito, habilitado ou desabilitado. O [**VideoStabilizationEffectEnabledChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn948979) tem um valor [**VideoStabilizationEffectEnabledChangedReason**](https://msdn.microsoft.com/library/windows/apps/dn948981) que indica por que o efeito foi habilitado ou desabilitado. Observe que esse evento também será gerado se você habilitá-lo ou desabilitá-lo o efeito programaticamente, nesse caso, o motivo será **Programmatic**.

Normalmente, você usaria esse evento para ajustar a interface do usuário do seu aplicativo para indicar o status atual de estabilização de vídeo.

[!code-cs[VideoStabilizationEnabledChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetVideoStabilizationEnabledChanged)]

### <a name="clean-up-the-video-stabilization-effect"></a>Limpar o efeito de estabilização de vídeo

Para limpar o efeito de estabilização de vídeo, chame [**RemoveEffectAsync**](https://msdn.microsoft.com/library/windows/apps/mt667957) para remover o efeito do pipeline do vídeo. Se as variáveis membro que contêm as propriedades de codificação iniciais não forem nulas, use-as para restaurar as propriedades de codificação. Por fim, remova o manipulador de eventos **EnabledChanged** e defina o efeito como "null".

[!code-cs[CleanUpVisualStabilizationEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetCleanUpVisualStabilizationEffect)]

## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Captura básica de fotos, áudio e vídeo com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




