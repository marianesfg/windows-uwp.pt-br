---
author: drewbatgit
ms.assetid: 42A06423-670F-4CCC-88B7-3DCEEDDEBA57
description: "Este artigo discute como usar perfis de câmera para descobrir e gerenciar as funcionalidades de diferentes dispositivos de captura de vídeo."
title: "Perfis de câmera"
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 755b2747b2250c4ad19970095aed220551389471

---

# Perfis de câmera

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este artigo discute como usar perfis de câmera para descobrir e gerenciar as funcionalidades de diferentes dispositivos de captura de vídeo.

**Observação**  
Este artigo se baseia em conceitos e códigos discutidos em [Capturar fotos e vídeos com o MediaCapture](capture-photos-and-video-with-mediacapture.md), que descreve as etapas para implementar uma captura básica de fotos e vídeos. É recomendável que você se familiarize com o padrão de captura de mídia básica neste artigo antes de passar para cenários de captura mais avançados. O código neste artigo presume que seu aplicativo já tenha uma instância de MediaCapture inicializada corretamente.

 

## Sobre perfis de câmera

As câmeras em diferentes dispositivos oferecem suporte a várias funcionalidades, inclusive ao conjunto de resoluções de captura com suporte, à taxa de quadros para capturas de vídeo ou a capturas de taxa de quadros variáveis ou HDR. A estrutura de captura de mídia da Plataforma Universal do Windows (UWP) armazena esse cojnunto de funcionalidades em uma [**MediaCaptureVideoProfileMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926695). Um perfil de câmera, representado por um objeto [**MediaCaptureVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926694), tem três conjuntos de descrições de mídia: um para captura de fotos, um para captura de vídeos e outro para visuaçização de vídeos.

Antes de inicializar seu objeto [MediaCapture](capture-photos-and-video-with-mediacapture.md), você pode consultar os dispositivos de captura no dispositivo atual para saber quais perfis têm suporte. Ao selecionar um perfil com suporte, você sabe que o dispositivo de captura oferece suporte a todas as funcionalidades nas descrições de mídia do perfil. Isso elimina a necessidade de uma abordagem de tentativa e erro para determinar quais combinações de funcionalidades têm suporte em um determinado dispositivo.

No artigo sobre captura de mídia básica, [Capturar fotos e vídeos com o MediaCapture](capture-photos-and-video-with-mediacapture.md), a [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573) usada para inicializar a captura de mídia é criada somente com a cadeia de caracteres da ID do dispositivo de captura, a quantidade mínima de dados necessária para a inicialização.

[!code-cs[BasicInitExample](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetBasicInitExample)]

Os exemplos de código neste artigo substituem essa inicialização mínima pela descoberta de perfis de câmera que oferecem suporte a diversas funcionalidades, que são usados para inicializar o dispositivo de captura de mídia.

## Encontre um dispositivo de vídeo que ofereça suporte a perfis de câmera

Antes de procurar perfis de câmera com suporte, você deve encontrar um dispositivo de captura que ofereça suporte ao uso de perfis de câmera. O método auxiliar **GetVideoProfileSupportedDeviceIdAsync** definido no exemplo a seguir usa o método [**DeviceInformaion.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) para recuperar uma lista de todos os dispositivos de captura de vídeo disponíveis. Ele executa um loop em todos os dispositivos na lista, chamando o método estático, [**IsVideoProfileSupported**](https://msdn.microsoft.com/library/windows/apps/dn926714), para cada dispositivo a fim de verificar se ele oferece suporte a perfis de vídeo. Também há a propriedade [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906) para cada dispositivo, que permite especificar se você deseja uma câmera na frente ou atrás do dispositivo.

Se um dispositivo que oferece suporte a perfis de câmera for encontrado no painel especificado, o valor [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437), que contém a cadeia de caracteres da ID do dispositivo, será retornado.

[!code-cs[GetVideoProfileSupportedDeviceIdAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetVideoProfileSupportedDeviceIdAsync)]

Se a ID do dispositivo retornada do método auxiliar **GetVideoProfileSupportedDeviceIdAsync** for uma cadeia de caracteres nula ou vazia, não há nenhum dispositivo no painel especificado que ofereça suporte a perfis de câmera. Nesse caso, você deve inicializar o dispositivo de captura de mídia sem usar perfis.

[!code-cs[GetDeviceWithProfileSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetDeviceWithProfileSupport)]

## Selecionar um perfil com base na resolução e na taxa de quadros com suporte

Para selecionar um perfil com determinadas funcionalidades, como a capacidade de alcançar uma determinada resolução e uma taxa de quadros, você deve primeiro chamar o método auxiliar definido acima para obter a ID de um dispositivo de captura que ofereça suporte ao uso de perfil de câmeras.

Crie um novo objeto [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573). Para isso, transmita a ID do dispositivo selecionado. Em seguida, chame o método estático [**MediaCapture.FindAllVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926708) para obter uma lista de todos os perfis de câmera compatíveis com o dispositivo.

Este exemplo usa um método de consulta Linq, incluído no namespace **System.Linq** em uso, para selecionar um perfil que contenha um objeto [**SupportedRecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926705) em que as propriedades [**Width**](https://msdn.microsoft.com/library/windows/apps/dn926700), [**Height**](https://msdn.microsoft.com/library/windows/apps/dn926697) e [**FrameRate**](https://msdn.microsoft.com/library/windows/apps/dn926696) correspondam aos valores solicitados. Se uma correspondência for encontrada, [**VideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926679) e [**RecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926678) de **MediaCaptureInitializationSettings** serão definidos como os valores do tipo anônimo retornado na consulta Linq. Se nenhuma correspondência for encontrada, o perfil padrão será usado.

[!code-cs[FindWVGA30FPSProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindWVGA30FPSProfile)]

Depois que você preencher **MediaCaptureInitializationSettings** com o perfil de câmera desejado, basta chamar [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) no objeto de captura de mídia para configurá-lo com o perfil desejado.

[!code-cs[InitCaptureWithProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitCaptureWithProfile)]

## Selecionar um perfil que ofereça suporte à simultaneidade

Você pode usar perfis de câmera para determinar se um dispositivo oferece suporte à captura de vídeo de várias câmeras simultaneamente. Para esse cenário, será necessário criar dois conjuntos de objetos de captura: um para a câmera frontal e outro para a câmera traseira. Para cada câmera, crie **MediaCapture**, **MediaCaptureInitializationSettings** e uma cadeia de caracteres para manter a ID do dispositivo de captura. Além disso, adicione uma variável booliana que detectará se a simultaneidade tem ou não suporte.

[!code-cs[ConcurrencySetup](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConcurrencySetup)]

O método estático [**MediaCapture.FindConcurrentProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926709) retorna uma lista dos perfis de câmera compatíveis com o dispositivo de captura especificado que também oferece suporte à simultaneidade. Use uma consulta Linq para encontrar um perfil que ofereça suporte à simultaneidade e seja compatível com as câmeras frontal e traseira. Se um perfil que atenda a esses requisitos for encontrado, defina o perfil em cada um dos objetos **MediaCaptureInitializationSettings** e defina a variável booliana de detecção de simultaneidade como true.

[!code-cs[FindConcurrencyDevices](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindConcurrencyDevices)]

Chame **MediaCapture.InitializeAsync** para a câmera principal do cenário de seu aplicativo. Se houver suporte para a simultaneidade, inicialize também a segunda câmera.

[!code-cs[InitConcurrentMediaCaptures](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitConcurrentMediaCaptures)]

## Usar perfis conhecidos para encontrar um perfil com suporte a vídeo HDR

A seleção de um perfil que ofereça suporte a HDR é feita como a de outros cenários. Crie **MediaCaptureInitializationSettings** e uma cadeia de caracteres para conter a ID do dispositivo de captura. Adicione uma variável booliana que detectará se o vídeo HDR tem suporte.

[!code-cs[GetHdrProfileSetup](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetHdrProfileSetup)]

Use o método auxiliar **GetVideoProfileSupportedDeviceIdAsync** definido acima para obter a ID de um dispositivo de captura com suporte a perfis de câmera.

[!code-cs[FindDeviceHDR](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindDeviceHDR)]

O método estático [**MediaCapture.FindKnownVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926710) retorna perfis de câmera compatíveis com o dispositivo especificado que é categorizado pelo valor [**KnownVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn948843) especificado. Para esse cenário, o valor **VideoRecording** é especificado para limitar os perfis de câmera retornados àqueles que ofereçam suporte à gravação de vídeo.

Execute um loop pela lista retornada de perfis de câmera. Para cada perfil de câmera, execute um loop em cada [**VideoProfileMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926695) no perfil, verificando se a propriedade [**IsHdrVideoSupported**](https://msdn.microsoft.com/library/windows/apps/dn926698) é true. Depois de encontrar uma descrição de mídia adequada, interrompa o loop e atribua o perfil e os objetos de descrição ao objeto **MediaCaptureInitializationSettings**.

[!code-cs[FindHDRProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindHDRProfile)]

## Determinar se um dispositivo oferece suporte à captura simultânea de fotos e vídeos

Muitos dispositivos oferecem suporte à captura simultânea de fotos e vídeos. Para determinar se um dispositivo de captura oferece esse suporte, chame [**MediaCapture.FindAllVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926708) para obter todos os perfis de câmera compatíveis com o dispositivo. Use a consulta de link para encontrar um perfil que tenha pelo menos uma entrada para [**SupportedPhotoMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926703) e [**SupportedRecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926705), o que significa que o perfil oferece suporte à captura simultânea.

[!code-cs[GetPhotoAndVideoSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPhotoAndVideoSupport)]

Você pode refinar essa consulta para procurar perfis com suporte a resoluções específicas ou outras funcionalidades, além da gravação simultânea de vídeo. Você também pode usar [**MediaCapture.FindKnownVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926710) e especificar o valor [**BalancedVideoAndPhoto**](https://msdn.microsoft.com/library/windows/apps/dn948843) para recuperar perfis com suporte à captura simultânea, mas realizar a consulta de todos os perfis fornecerá resultados mais completos.

## Tópicos relacionados

* [Capturar fotos e vídeos com o MediaCapture](capture-photos-and-video-with-mediacapture.md)
 

 







<!--HONumber=Jun16_HO4-->


