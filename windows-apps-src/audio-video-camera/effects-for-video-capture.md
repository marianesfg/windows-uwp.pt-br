---
author: drewbatgit
ms.assetid: E0189423-1DF3-4052-AB2E-846EA18254C4
description: "Este tópico descreve os efeitos projetados para uso em cenários de captura de vídeo. Isso inclui o efeito de estabilização de vídeo."
title: "Efeitos para captura de vídeo"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 3af5ed7146f2420c2a6d3035c26290cbeaff8375

---

# Efeitos para captura de vídeo

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este tópico descreve os efeitos projetados para uso em cenários de captura de vídeo. Isso inclui o efeito de estabilização de vídeo.

**Observação**  
Este artigo se baseia em conceitos e códigos discutidos em [Capturar fotos e vídeos com o MediaCapture](capture-photos-and-video-with-mediacapture.md), que descreve as etapas para implementar uma captura básica de fotos e vídeos. É recomendável que você se familiarize com o padrão de captura de mídia básica neste artigo antes de passar para cenários de captura mais avançados. O código deste artigo presume que o seu aplicativo já tenha uma instância do MediaCapture inicializada corretamente.

## Efeito de estabilização de vídeo

O efeito de estabilização de vídeo manipula os quadros de um fluxo de vídeo para minimizar a vibração causada por segurar o dispositivo de captura em sua mão. Como essa técnica faz com que os pixels sejam deslocados para a direita, para a esquerda, para cima e para baixo, e como o efeito identifica o conteúdo fora do quadro de vídeo, o vídeo estabilizado é cortado ligeiramente do vídeo original. Uma função utilitária é fornecida para permitir que você ajuste suas configurações de codificação para gerenciar de forma ideal o corte realizado pelo efeito.

Nos dispositivos com suporte para isso, a OIS (estabilização de imagem ótica) estabiliza o vídeo manipulando mecanicamente o dispositivo de captura e, portanto, não precisa cortar as bordas dos quadros do vídeo. Para obter mais informações, consulte [Controles de captura do dispositivo para a captura de vídeo](capture-device-controls-for-video-capture.md).

### Configurar seu aplicativo para usar estabilização de vídeo

Além dos namespaces necessários para captura de mídia básica, o uso do efeito de estabilização de vídeo exige o namespace a seguir.

[!code-cs[VideoStabilizationEffectUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoStabilizationEffectUsing)]

Declare uma variável membro para armazenar o objeto [**VideoStabilizationEffect**](https://msdn.microsoft.com/library/windows/apps/dn926760). Como parte da implementação do efeito, você modificará as propriedades de codificação que usa para codificar o vídeo capturado. Declare duas variáveis para armazenar uma cópia de backup das propriedades de codificação de entrada e saída para poder restaurá-las mais tarde, quando o efeito estiver desabilitado. Por fim, declare uma variável membro do tipo [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026), pois esse objeto será acessado em vários locais dentro do seu código.

[!code-cs[DeclareVideoStabilizationEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareVideoStabilizationEffect)]

Na implementação básica de captura de vídeo descrita no artigo [Capturar fotos e vídeos com o MediaCapture](capture-photos-and-video-with-mediacapture.md), o objeto de perfil de codificação de mídia é atribuído a uma variável local porque não é usado em outro lugar no código. Para esse cenário, você deve atribuir o objeto a uma variável membro para poder acessá-lo posteriormente.

[!code-cs[EncodingProfileMember](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEncodingProfileMember)]

### Inicializar o efeito de estabilização de vídeo

Após a inicialização do seu objeto **MediaCapture**, crie uma nova instância do objeto [**VideoStabilizationEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn926762). Chame [**MediaCapture.AddVideoEffectAsync**](https://msdn.microsoft.com/library/windows/apps/dn878035) para adicionar o efeito ao pipeline de vídeo e recuperar uma instância da classe [**VideoStabilizationEffect**](https://msdn.microsoft.com/library/windows/apps/dn926760). Especifique [**MediaStreamType.VideoRecord**](https://msdn.microsoft.com/library/windows/apps/br226640) para indicar que o efeito deve ser aplicado ao fluxo de gravação de vídeo.

Registre um manipulador de eventos para o evento [**EnabledChanged**](https://msdn.microsoft.com/library/windows/apps/dn948982) e chame o método auxiliar **SetUpVideoStabilizationRecommendationAsync**, ambos discutidos mais adiante neste artigo. Por fim, defina a propriedade [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926775) do efeito como "true" para habilitá-lo.

[!code-cs[CreateVideoStabilizationEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateVideoStabilizationEffect)]

### Usar as propriedades de codificação recomendadas

Conforme discutido anteriormente neste artigo, a técnica usada pelo efeito de estabilização de vídeo faz inevitavelmente com que o vídeo estabilizado seja cortado em uma certa medida da fonte de vídeo. Defina a função de auxiliar a seguir em seu código para ajustar as propriedades de codificação de vídeo para lidar do modo ideal com essa limitação do efeito. Esta etapa não é necessária para usar o efeito de estabilização de vídeo, mas, se você não executá-la, o vídeo resultante será ampliado um pouco e, portanto, terá uma fidelidade visual um pouco menor.

Chame [**GetRecommendedStreamConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn948983) na sua instância de efeito de estabilização de vídeo, transmitindo o objeto [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825), que informa o efeito sobre as suas propriedades de codificação de fluxo de entrada atuais, e seu **MediaEncodingProfile**, que faz com que o evento conheça as suas propriedades de codificação de saída atuais. Esse método retorna um objeto [**VideoStreamConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn926727) contendo novas propriedades de codificação de fluxo de entrada e saída recomendadas.

As propriedades recomendadas de codificação de entrada apresentam, caso o dispositivo ofereça suporte, uma resolução mais alta do que as configurações iniciais que você forneceu, para que haja o mínimo de perda na resolução após a aplicação do efeito de corte.

Chame [**VideoDeviceController.SetMediaStreamPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh700895) para definir as novas propriedades de codificação. Antes de definir as novas propriedades, use a variável membro para armazenar as propriedades de codificação iniciais para que você possa modificar as configurações de novo quando desabilitar o efeito.

Se o efeito de estabilização de vídeo precisar cortar o vídeo de saída, as propriedades de codificação de saída recomendadas serão do tamanho do vídeo cortado. Isso significa que a resolução de saída corresponderá ao tamanho do vídeo cortado. Se você não usar as propriedades de saída recomendadas, o vídeo será dimensionado para corresponder ao tamanho de saída inicial, o que resultará em uma perda de fidelidade visual.

Defina a propriedade [**Video**](https://msdn.microsoft.com/library/windows/apps/hh701124) do objeto **MediaEncodingProfile**. Antes de definir as novas propriedades, use a variável membro para armazenar as propriedades de codificação iniciais para que você possa modificar as configurações de novo quando desabilitar o efeito.

[!code-cs[SetUpVideoStabilizationRecommendationAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetUpVideoStabilizationRecommendationAsync)]

### Manipular o efeito de estabilização de vídeo que está sendo desabilitado

O sistema poderá desabilitar automaticamente o efeito de estabilização de vídeo se a taxa de transferência de pixel for muito alta para o efeito ou se detectar que o efeito está sendo executado lentamente. Se isso ocorrer, o evento EnabledChanged será gerado. A instância **VideoStabilizationEffect** no parâmetro *sender* indica o novo estado do efeito, habilitado ou desabilitado. O [**VideoStabilizationEffectEnabledChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn948979) tem um valor [**VideoStabilizationEffectEnabledChangedReason**](https://msdn.microsoft.com/library/windows/apps/dn948981) que indica por que o efeito foi habilitado ou desabilitado. Observe que esse evento também será gerado se você habilitá-lo ou desabilitá-lo o efeito programaticamente, nesse caso, o motivo será **Programmatic**.

Normalmente, você usaria esse evento para ajustar a interface do usuário do seu aplicativo para indicar o status atual de estabilização de vídeo.

[!code-cs[VideoStabilizationEnabledChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoStabilizationEnabledChanged)]

### Limpar o efeito de estabilização de vídeo

Para limpar o efeito de estabilização de vídeo, chame [**ClearEffectsAsync**](https://msdn.microsoft.com/library/windows/apps/br226592) para limpar todos os efeitos do pipeline do vídeo. Se as variáveis membro que contêm as propriedades de codificação iniciais não forem nulas, use-as para restaurar as propriedades de codificação. Por fim, remova o manipulador de eventos **EnabledChanged** e defina o efeito como "null".

[!code-cs[CleanUpVisualStabilizationEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpVisualStabilizationEffect)]

## Tópicos relacionados

* [Capturar fotos e vídeos com o MediaCapture](capture-photos-and-video-with-mediacapture.md)
 

 







<!--HONumber=Jun16_HO4-->


