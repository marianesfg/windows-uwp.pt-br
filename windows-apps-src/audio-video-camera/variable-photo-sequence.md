---
author: drewbatgit
ms.assetid: 7DBEE5E2-C3EC-4305-823D-9095C761A1CD
description: Este artigo mostra como capturar uma sequência de fotos variável que permite que você capture vários quadros de imagem em sucessão rápida e configure cada quadro para usar diferentes configurações de foco, flash, ISO, exposição e compensação de exposição.
title: Sequência de fotos variável
---

# Sequência de fotos variável

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este artigo mostra como capturar uma sequência de fotos variável que permite que você capture vários quadros de imagem em sucessão rápida e configure cada quadro para usar diferentes configurações de foco, flash, ISO, exposição e compensação de exposição. Esse recurso habilita cenários como criar imagens em HDR (High Dynamic Range).

Se você deseja capturar imagens em HDR, mas não deseja implementar seu próprio algoritmo de processamento, use a API [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) para utilizar os recursos HDR nativos do Windows. Para obter mais informações, consulte [Captura de fotos em HDR (High Dynamic Range)](high-dynamic-range-hdr-photo-capture.md).

**Observação**  
Este artigo se baseia em conceitos e códigos discutidos em [Capturar fotos e vídeos com o MediaCapture](capture-photos-and-video-with-mediacapture.md), que descreve as etapas para implementar uma captura básica de fotos e vídeos. É recomendável que você se familiarize com o padrão de captura de mídia básica neste artigo antes de passar para cenários de captura mais avançados. O código deste artigo presume que seu aplicativo já tenha uma instância de MediaCapture inicializada corretamente.

## Configurar seu aplicativo para usar a captura de sequência de fotos variável

Além dos namespaces necessários para captura de mídia básica, implementar uma captura de sequência de fotos variável exige os namespaces a seguir.

[!code-cs[VPSUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSUsing)]

Declare uma variável de membro para armazenar o objeto [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564), que é usado para iniciar a captura de sequência de fotos. Declare uma matriz de objetos [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) para armazenar cada imagem capturada na sequência. Além disso, declare uma matriz para armazenar o objeto [**CapturedFrameControlValues**](https://msdn.microsoft.com/library/windows/apps/dn608020) para cada quadro. Isso pode ser usado pelo seu algoritmo de processamento de imagem para determinar quais configurações foram usadas para capturar cada quadro. Por fim, declare um índice que será usado para controlar a imagem que está sendo capturada na sequência.

[!code-cs[VPSMemberVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSMemberVariables)]

## Preparar a captura de sequência de fotos variável

Depois de iniciar seu [MediaCapture](capture-photos-and-video-with-mediacapture.md), confira se as sequências de fotos variáveis têm suporte no dispositivo atual por meio da obtenção de uma instância do [**VariablePhotoSequenceController**](https://msdn.microsoft.com/library/windows/apps/dn640573) a partir do [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) da captura de mídia e da verificação da propriedade [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn640580).

[!code-cs[IsVPSSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsVPSSupported)]

Obtenha um objeto [**FrameControlCapabilities**](https://msdn.microsoft.com/library/windows/apps/dn652548) a partir do controlador de sequência de fotos variável. Esse objeto tem uma propriedade para cada configuração que pode ser definida por quadro de uma sequência de fotos. São elas:

-   [**Exposição**](https://msdn.microsoft.com/library/windows/apps/dn652552)
-   [**ExposureCompensation**](https://msdn.microsoft.com/library/windows/apps/dn652560)
-   [**Flash**](https://msdn.microsoft.com/library/windows/apps/dn652566)
-   [**Foco**](https://msdn.microsoft.com/library/windows/apps/dn652570)
-   [**IsoSpeed**](https://msdn.microsoft.com/library/windows/apps/dn652574)
-   [**PhotoConfirmation**](https://msdn.microsoft.com/library/windows/apps/dn652578)

Este exemplo definirá um valor de compensação de exposição diferente para cada quadro. Para verificar se a compensação de exposição tem suporte para sequências de fotos no dispositivo atual, verifique a propriedade [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn278905) do objeto [**FrameExposureCompensationCapabilities**](https://msdn.microsoft.com/library/windows/apps/dn652628) acessado por meio da propriedade **ExposureCompensation**.

[!code-cs[IsExposureCompensationSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsExposureCompensationSupported)]

Crie um novo objeto [**FrameController**](https://msdn.microsoft.com/library/windows/apps/dn652582) para cada quadro que você deseja capturar. Este exemplo captura três quadros. Defina os valores para os controles que você deseja variar para cada quadro. Em seguida, desmarque a coleção [**DesiredFrameControllers**](https://msdn.microsoft.com/library/windows/apps/dn640574) do **VariablePhotoSequenceController** e adicione o controlador de cada quadro à coleção.

[!code-cs[InitFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitFrameControllers)]

Crie um objeto [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) para definir a codificação que você deseja usar para as imagens capturadas. Chame o método estático [**MediaCapture.PrepareVariablePhotoSequenceCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/dn608097), por meio da transmissão das propriedades codificadas. Este método retorna um objeto [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564). Por fim, registre manipuladores de eventos para os eventos [**PhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/dn652573) e [**Stopped**](https://msdn.microsoft.com/library/windows/apps/dn652585).

[!code-cs[PrepareVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPrepareVPS)]

## Iniciar a captura de sequência de fotos variável

Para iniciar a captura de sequência de fotos variável, chame [**VariablePhotoSequenceCapture.StartAsync**](https://msdn.microsoft.com/library/windows/apps/dn652577). Assegure-se de inicializar as matrizes para armazenar os valores de controle de quadro e as imagens capturadas. Defina o índice atual como 0. Defina a variável de estado de gravação de seu aplicativo e atualize sua interface do usuário para desabilitar a inicialização de outra captura enquanto uma captura está em andamento.

[!code-cs[StartVPSCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartVPSCapture)]

## Receber os quadros capturados

O evento [**PhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/dn652573) é gerado para cada quadro capturado. Salve os valores de controle de quadro e a imagem capturada do quadro. Em seguida, incremente o índice de quadro atual. Este exemplo mostra como obter uma representação [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) de cada quadro. Para obter mais informações sobre o uso de **SoftwareBitmap**, consulte [Geração de imagens](imaging.md).

[!code-cs[OnPhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnPhotoCaptured)]

## Manipular a conclusão da captura de sequência de fotos variável

O evento [**Stopped**](https://msdn.microsoft.com/library/windows/apps/dn652585) é gerado quando todos os quadros na sequência forem capturados. Atualize o estado de gravação do seu aplicativo e sua interface do usuário para permitir que o usuário inicie novas capturas. Nesse ponto, você pode passar os valores de controle de quadro e as imagens capturadas a seu código de processamento de imagem.

[!code-cs[OnStopped](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnStopped)]

## Atualizar controladores de quadro

Se você deseja executar outra captura de sequência de fotos variável com diferentes configurações por quadro, não é necessário reinicializar a **VariablePhotoSequenceCapture**. Você pode desmarcar a coleção [**DesiredFrameControllers**](https://msdn.microsoft.com/library/windows/apps/dn640574) e adicionar novos controladores de quadro ou pode modificar os valores de controlador de quadro existentes. Os exemplos a seguir conferem o objeto [**FrameFlashCapabilities**](https://msdn.microsoft.com/library/windows/apps/dn652657) para verificar se o dispositivo atual suporta energia flash e flash para quadros de sequência de fotos variável. Nesse caso, cada quadro é atualizado para habilitar o flash em 100% de energia. Os valores de compensação de exposição que já foram definidos para cada quadro ainda estão ativos.

[!code-cs[UpdateFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateFrameControllers)]

## Desmarcar a captura de sequência de fotos variável

Quando você terminar de capturar as sequências de fotos variáveis ou seu aplicativo estiver suspenso, desmarque o objeto da sequência de fotos variável. Basta chamar [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/dn652569). Cancele o registro de manipuladores de evento do objeto e defina-o como null.

[!code-cs[CleanUpVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpVPS)]

## Tópicos relacionados

* [Capturar fotos e vídeos com o MediaCapture](capture-photos-and-video-with-mediacapture.md)
 

 






<!--HONumber=May16_HO2-->


