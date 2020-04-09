---
ms.assetid: DE5B084C-DAC1-430B-A15B-5B3D5FB698F7
title: Otimizar animações, mídia e imagens
description: Crie aplicativos da Plataforma Universal do Windows (UWP) com animações suaves, taxa de quadros elevada e captura e reprodução de mídia de alto desempenho.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 579772bba55c93de38c3c43538ad14253dbc2572
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339890"
---
# <a name="optimize-animations-media-and-images"></a>Otimizar animações, mídia e imagens


Crie aplicativos da Plataforma Universal do Windows (UWP) com animações suaves, taxa de quadros elevada e captura e reprodução de mídia de alto desempenho.

## <a name="make-animations-smooth"></a>Torne as animações suaves

Um aspecto importante dos aplicativos UWP são as interações suaves. Isso inclui manipulações de toque que "aderem ao seu dedo", transições e animações suaves e pequenos movimentos que oferecem retroalimentação de entrada. Na estrutura XAML, existe um thread, chamado de thread de composição, que é dedicado à composição e à animação de elementos visuais de um aplicativo. Como o thread de composição é separado do thread de interface do usuário (o thread que executa o código de estrutura e de desenvolvedor), os aplicativos podem obter uma taxa de quadros consistente e animações suaves, independentemente de complicadas passagens de layout ou de cálculos estendidos. Esta seção mostra como usar o thread de composição para manter as animações de um aplicativo suaves. Para saber mais sobre animações, consulte [Visão geral de animações](https://docs.microsoft.com/windows/uwp/graphics/animations-overview). Para aprender a aumentar a capacidade de resposta de um aplicativo durante a execução de cálculos intensos, consulte [Manter o thread de interface do usuário respondendo](keep-the-ui-thread-responsive.md).

### <a name="use-independent-instead-of-dependent-animations"></a>Usar animações independentes em vez de dependentes

As animações independentes podem ser calculadas do início ao fim no momento da criação, pois as alterações na propriedade que está sendo animada não afetam o resto dos objetos em uma cena. As animações independentes podem, portanto, ser executadas no thread de composição em vez de no thread de interface do usuário. Isso garante que elas permanecerão suaves porque o thread de composição é atualizado em uma cadência consistente.

Todos estes tipos de animação são garantidamente independentes:

-   Animações de objeto usando quadros chave
-   Animações de duração zero
-   Animações para as propriedades [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left) e [**Canvas.Top**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.top)
-   Animações para a propriedade [**UIElement. Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)
-   Animações para propriedades do tipo [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) quando voltadas para a subpropriedade [**SolidColorBrush.Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color)
-   Animações para as seguintes propriedades [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) quando voltadas para subpropriedades destes tipos de valor de retorno:

    -   [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform)
    -   [**Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d)
    -   [**Projection**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.projection)
    -   [**Clip**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip)

Animações dependentes afetam o layout, que, portanto, não pode ser calculado sem entrada extra do thread de interface do usuário. As animações dependentes incluem modificações a propriedades como [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) e [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height). Por padrão, as animações dependentes não são executadas e exigem uma aceitação do desenvolvedor do aplicativo. Quando habilitadas, elas são executadas suavemente caso o thread de interface do usuário permaneça desbloqueado, mas começarão a tremer caso a estrutura ou o aplicativo esteja tendo muito trabalho no thread de interface do usuário.

Quase todas as animações na estrutura XAML são independentes por padrão, mas há algumas ações que você pode tomar para desabilitar essa otimização. Tenha cuidado com estas situações em especial:

-   Configurando a propriedade [**EnableDependentAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.pointanimation.enabledependentanimation) para permitir que uma animação dependente seja executada no thread de interface do usuário. Converter essas animações em uma versão independente. Por exemplo, anime [**ScaleTransform.ScaleX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.scalex) e [**ScaleTransform.ScaleY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.scaley), em vez de [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) e [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) de um objeto. Não tenha medo de dimensionar objetos como imagens e texto. A estrutura aplica escalas bilineares apenas enquanto o [**ScaleTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ScaleTransform) está sendo animado. A imagem/texto será rasterizada novamente no tamanho final para garantir que fique sempre clara.
-   Fazer atualizações por quadro, que são, efetivamente, animações dependentes. Um exemplo disso é a aplicação de transformações no manipulador do evento [**CompositonTarget.Rendering**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.compositiontarget.rendering).
-   Executando qualquer animação considerada independente em um elemento com a propriedade [**CacheMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.cachemode) definida para **BitmapCache**. Isso é considerado dependente porque o cache deve ser rasterizado novamente para cada quadro.

### <a name="dont-animate-a-webview-or-mediaplayerelement"></a>Não anime um WebView ou um MediaPlayerElement

O conteúdo Web dentro de um controle [**WebView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) não é renderizado diretamente pela estrutura XAML e requer trabalho extra para ser composto com o resto do cenário. Esse trabalho extra aumenta quando se está animando o controle em torno da tela e pode introduzir problemas de sincronização (por exemplo, o conteúdo HTML pode não se mover em sincronia com o resto do conteúdo XAML na página). Quando você precisar animar um controle **WebView**, alterne-o com um [**WebViewBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewbrush) pela duração da animação.

Animar um [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) também é má ideia. Além do detrimento de desempenho, isso pode causar ruptura ou outros artefatos no conteúdo de vídeo sendo reproduzido.

> **Observação**   As recomendações neste artigo para **MediaPlayerElement** também se aplicam a [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement). **MediaPlayerElement** só está disponível no Windows 10, versão 1607, portanto se você está criando um aplicativo para uma versão anterior do Windows, precisará usar o **MediaElement**.

### <a name="use-infinite-animations-sparingly"></a>Use animações infinitas com moderação

A maioria das animações é executada por uma quantidade de tempo especificada, mas definir a propriedade [**Timeline.Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration) para Forever permite que uma animação seja executada infinitamente. É recomendável minimizar o uso de animações infinitas porque elas consomem continuamente recursos de CPU e podem impedir a CPU de ir para um estado de pouca energia ou inativo, fazendo com que fique sem energia mais rapidamente.

Adicionar um manipulador para [**CompositionTarget.Rendering**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.compositiontarget.rendering) é similar a executar uma animação infinita. Normalmente, o thread de interface do usuário fica ativo apenas quando há trabalho para fazer, mas adicionar um manipulador para esse evento o força a executar cada quadro. Remova o manipulador quando não houver trabalho a ser feito e registre-o novamente quando for necessário de novo.

### <a name="use-the-animation-library"></a>Use a Biblioteca de Animação

O namespace [**Windows.UI.Xaml.Media.Animation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation) inclui uma biblioteca de animações suaves de alto desempenho que têm aspecto consistente com outras animações do Windows. As classes relevantes têm "Theme" no nome e estão descritas em [Visão geral de animações](https://docs.microsoft.com/windows/uwp/graphics/animations-overview). Esta biblioteca é compatível com vários cenários de animação comuns, como a animação da primeira exibição do aplicativo e a criação de transições de estado e conteúdo. É recomendável usar essa biblioteca de animação sempre que possível, para aumentar o desempenho e a consistência para a interface do usuário do UWP.

> **Observação**   A Biblioteca de Animação não pode animar todas as propriedades possíveis. Para todos os cenários XAML onde a biblioteca de animação não se aplica, consulte [Animações com storyboard](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations).


### <a name="animate-compositetransform3d-properties-independently"></a>Animar propriedades CompositeTransform3D independentemente

Você pode animar cada propriedade de uma [**CompositeTransform3D**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Media3D.CompositeTransform3D) independentemente, então aplique apenas as animações necessárias. Para obter mais informações e exemplos, consulte [**UIElement.Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d). Para saber mais sobre animação de transformações, consulte [Animações com storyboard](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations) e [Animações de quadro chave e animações com função de easing](https://docs.microsoft.com/windows/uwp/graphics/key-frame-and-easing-function-animations).

## <a name="optimize-media-resources"></a>Otimizar recursos de mídia

Áudio, vídeo e imagens são formas atraentes de conteúdo que a maioria dos aplicativos usa. À medida que as taxas de captura aumentam e o conteúdo se move de definição padrão para alta definição, a quantidade de recursos necessários para armazenar, decodificar e reproduzir esse conteúdo aumenta. A estrutura XAML utiliza os recursos mais recentes adicionados aos mecanismos de mídia da UWP, de forma que os aplicativos obtenham esses aprimoramentos gratuitamente. Aqui, explicamos alguns truques adicionais que permitem que você aproveite ao máximo a mídia em seu aplicativo UWP.

### <a name="release-media-streams"></a>Liberar fluxos de mídia

Os arquivos de mídia são alguns dos recursos mais comuns e mais caros que os aplicativos geralmente usam. Como os recursos de arquivos de mídia podem aumentar significativamente o tamanho do volume de memória de seu aplicativo, você precisa se lembrar de liberar o manipulador para mídia assim que o aplicativo terminar de usá-lo.

Por exemplo, se seu aplicativo trabalha com um objeto [**RandomAccessStream**](/uwp/api/Windows.Storage.Streams.RandomAccessStream) ou [**IInputStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IInputStream), certifique-se de chamar o método de encerramento no objeto quando o aplicativo terminar de usá-lo, para liberar o objeto subjacente.

### <a name="display-full-screen-video-playback-when-possible"></a>Exiba reprodução de vídeo em tela cheia quando possível.

Em aplicativos UWP, sempre use a propriedade [**IsFullWindow**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.isfullwindow) no [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) para habilitar e desabilitar a renderização em janela maximizada. Isso garante que otimizações de nível de sistema sejam usadas durante a reprodução de mídia.

A estrutura XAML pode otimizar a exibição de conteúdo de vídeo quando é a única coisa sendo renderizada, resultando em uma experiência que usa menos energia e rende taxas de quadros mais elevadas. Para uma reprodução de mídia mais eficiente, configure o tamanho de um **MediaPlayerElement** para ser a largura e a altura da tela e não exiba outros elementos XAML

Há razões legítimas para sobrepor elementos XAML em um **MediaPlayerElement** que assume a largura e a altura totais da tela, por exemplo, legenda oculta ou controles de transporte momentâneos. Certifique-se de ocultar esses elementos (definir `Visibility="Collapsed"`) quando não são necessários para recolocar reprodução de mídia em seu estado mais eficiente.

### <a name="display-deactivation-and-conserving-power"></a>Desativação da exibição e economia de energia

Para evitar que a exibição seja desativada quando não for mais detectada atividade de usuário, como quando um aplicativo estiver reproduzindo um vídeo, você pode chamar [**DisplayRequest.RequestActive**](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestactive).

Para economizar energia e duração da bateria, você deve chamar [**DisplayRequest.RequestRelease**](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestrelease) para liberar a solicitação de exibição assim que não for mais necessária.

Consulte algumas situações em que você deve liberar a solicitação de exibição:

-   Por exemplo, a reprodução do vídeo foi pausada por uma ação do usuário, buffer ou ajuste devido à largura de banda limitada.
-   A reprodução foi interrompida. Por exemplo, a reprodução do vídeo ou a apresentação acabou.
-   Erro na reprodução. Por exemplo, problemas com a conectividade de rede ou um arquivo corrompido.

### <a name="put-other-elements-to-the-side-of-embedded-video"></a>Colocar outros elementos ao lado de vídeo incorporado

Com frequência, aplicativos oferecem uma exibição incorporada onde o vídeo é exibido em uma página. Agora, você obviamente perdeu a otimização em tela cheia, porque o [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) não tem o tamanho da página e há outros objetos XAML desenhados. Cuidado para não entrar sem querer nesse modo desenhando uma margem em torno de um **MediaPlayerElement**.

Não desenhe elementos XAML por cima do vídeo quando estiver em modo incorporado. Se fizer isso, a estrutura será forçada a fazer um trabalho extra para compor a cena. Posicionar controles de transporte abaixo de um elemento de mídia incorporado em vez de em cima do vídeo é um bom exemplo de otimização para esta situação. Nesta imagem, a barra vermelha indica um conjunto de transportes de controle (reproduzir, pausar, parar etc.).

![MediaPlayerElement com elementos sobrepostos](images/videowithoverlay.png)

Não coloque estes controles em cima de mídia que não esteja em tela cheia. Em vez disso, posicione os controles de transporte em algum lugar fora da área onde a mídia estiver sendo renderizada. Na próxima imagem, os controles são colocados abaixo da mídia.

![MediaPlayerElement com elementos vizinhos](images/videowithneighbors.png)

### <a name="delay-setting-the-source-for-a-mediaplayerelement"></a>Atrasar a definição da fonte de um MediaPlayerElement

Os mecanismos de mídia são objetos caros e a estrutura XAML atrasa o carregamento de DLLs e a criação de objetos maiores o quanto for possível. [  **MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) é forçado a fazer esse trabalho depois que sua origem é definida por meio da propriedade [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source). Configurá-lo quando o usuário estiver realmente pronto para executar mídia atrasa a maioria do custo associado com o **MediaPlayerElement** o quanto for possível.

### <a name="set-mediaplayerelementpostersource"></a>Definir MediaPlayerElement.PosterSource

A configuração do [**MediaPlayerElement.PosterSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.postersource) permite que o XAML libere alguns recursos GPU que, caso contrário, teriam sido usados. Essa API permite que um aplicativo use o mínimo de memória possível.

### <a name="improve-media-scrubbing"></a>Aprimorar a depuração de mídia

É sempre difícil fazer com que a limpeza seja uma tarefa com alta capacidade de resposta em plataformas de mídia. Geralmente, as pessoas conseguem fazer isso alterando o valor de um Controle Deslizante. Aqui estão duas dicas sobre como deixar isso o mais eficiente possível:

-   Atualize o valor do [**Controle deslizante**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) com base em um temporizador que consulta a [**Posição**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.position) no [**MediaPlayerElement.MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer). Certifique-se de usar uma frequência de atualização sensata para seu temporizador. A propriedade **Posição** atualiza apenas a cada 250 milissegundos durante a reprodução.
-   O tamanho da frequência de etapa no Controle Deslizante deve escalar com o comprimento do vídeo.
-   Assine os eventos [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed), [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved), [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased) no controle deslizante para configurar a propriedade [**PlaybackRate**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.playbackrate) como 0 quando o usuário arrastar o elevador do controle deslizante.
-   No manipulador de evento [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased), configure manualmente a posição de mídia para o valor de posição do controle deslizante para alcançar ótima retenção do elevador enquanto arrasta.

### <a name="match-video-resolution-to-device-resolution"></a>Corresponder a resolução do vídeo à resolução do dispositivo

A decodificação de vídeo faz uso intensivo de memória e de ciclos de GPU e, portanto, escolha um formato de vídeo próximo à resolução em que ele será exibido. Não faz sentido usar os recursos para decodificar um vídeo de 1080 se ele será dimensionado para um tamanho muito menor. Muitos aplicativos não têm o mesmo vídeo codificado em diferentes resoluções, mas se ele estiver disponível, use uma codificação que esteja próxima à que ele será exibido.

### <a name="choose-recommended-formats"></a>Escolher formatos recomendados

A seleção de formatos de mídia pode ser complicada e, com frequência, ela é motivada por decisões de negócios. De uma perspectiva de desempenho da UWP, recomendamos usar vídeos H.264 como o principal formato de vídeo e AAC e MP3 como os formatos de áudio preferidos. Para reproduzir arquivos locais, MP4 é o contêiner de arquivo recomendado para conteúdo de vídeo. A decodificação H.264 é acelerada por meio do hardware gráfico mais recente. Além disso, apesar de a aceleração de hardware para a decodificação de VC-1 estar amplamente disponível, para uma grande quantidade dos hardwares gráficos no mercado, a aceleração é limitada a um nível parcial (ou nível de IDCT) em vários casos, não um descarregamento de hardware em nível total (ou seja, o modo VLD).

Se você tiver controle total do processo de geração do conteúdo do vídeo, deverá descobrir como manter um bom equilíbrio entre a eficiência da compactação e a estrutura GOP. Um tamanho de GOP relativamente menor, com imagens B, pode aumentar o desempenho nos modos de busca ou de truque.

Ao incluir efeitos de áudio curtos de baixa latência, por exemplo, em jogos, use arquivos WAV com dados PCM compactados para reduzir a sobrecarga de processamento típica para formatos de áudio compactados.


## <a name="optimize-image-resources"></a>Otimizar recursos de imagem

### <a name="scale-images-to-the-appropriate-size"></a>Dimensionar imagens para o tamanho adequado

As imagens são capturadas em resoluções muito altas, o que pode resultar em aplicativos usando mais recursos de CPU ao decodificar os dados da imagem e mais memória depois que ela é carregada do disco. Porém, não faz sentido decodificar e salvar uma imagem de alta resolução na memória somente para exibi-la em um tamanho menor do que o nativo. Em vez disso, crie uma versão da imagem no tamanho exato que ela será desenhada na tela usando as propriedades [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) e [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight).

Não faça isto:

```xaml
<Image Source="ms-appx:///Assets/highresCar.jpg"
       Width="300" Height="200"/>    <!-- BAD CODE DO NOT USE.-->
```

Em vez disso, faça o seguinte:

```xaml
<Image>
    <Image.Source>
    <BitmapImage UriSource="ms-appx:///Assets/highresCar.jpg"
                 DecodePixelWidth="300" DecodePixelHeight="200"/>
    </Image.Source>
</Image>
```

As unidades para [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) e [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight) são, por padrão, pixels físicos. A propriedade [**DecodePixelType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixeltype) pode ser usada para alterar esse comportamento: definir **DecodePixelType** como **Logical** resulta no tamanho de decodificação respondendo automaticamente pelo fator de escala atual do sistema, similar a outros conteúdos XAML. Portanto, em geral, seria adequado definir **DecodePixelType** como **Logical**, se, por exemplo, você quiser que **DecodePixelWidth** e **DecodePixelHeight** correspondam às propriedades de altura e largura do controle de imagem em que a imagem será exibida. Com o comportamento padrão de uso de pixels físicos, você deve considerar o fator de escala atual do sistema por conta própria; e deve detectar as notificações de alteração de escala no caso de o usuário alterar suas preferências de exibição.

Se DecodePixelWidth/Height forem configurados explicitamente maiores do que a imagem que será exibida na tela, então o aplicativo vai, desnecessariamente, usar memória extra, até 4 bytes por pixel, o que se torna rapidamente caro para imagens grandes. A imagem também será dimensionada para baixo usando dimensionamento bilinear, o que pode fazer com que ela pareça embaçada para fatores de escala grandes.

Se DecodePixelWidth/DecodePixelHeight forem definidas explicitamente menores do que a imagem que será exibida na tela, ela será expandida e poderá parecer pixelada.

Em alguns casos em que um tamanho de decodificação apropriado não puder ser determinado antecipadamente, você deverá consultar a decodificação automática de tamanho correto do XAML, que fará o máximo para tentar decodificar a imagem no tamanho apropriado se um DecodePixelWidth/DecodePixelHeight explícito não for especificado.

Você deve definir um tamanho de decodificação explícito, se souber o tamanho do conteúdo da imagem com antecedência. Você também deve definir, juntamente, [**DecodePixelType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixeltype) como **Logical**, se o tamanho de decodificação fornecido for relativo a outros tamanhos de elemento XAML. Por exemplo, se você definir explicitamente o tamanho do conteúdo com Image.Width e Image.Height, poderá definir DecodePixelType como DecodePixelType.Logical para usar as mesmas dimensões de pixel lógico como um controle Image e, em seguida, usar explicitamente BitmapImage.DecodePixelWidth e/ou BitmapImage.DecodePixelHeight para controlar o tamanho da imagem para obter uma economia de memória potencialmente grande.

Observe que Image.Stretch devem ser considerado ao determinar o tamanho do conteúdo decodificado.

### <a name="right-sized-decoding"></a>Decodificação de tamanho correto

Caso você não configure um tamanho de decodificação explícito, o XAML fará uma tentativa de melhor esforço para economizar memória decodificando uma imagem ao tamanho exato que ela aparecerá na tela de acordo com o layout inicial da página relativa. É recomendável gravar seu aplicativo de forma que possa usar esse recurso quando possível. Esse recurso será desativado se qualquer uma das seguintes condições for atendida.

-   O [**BitmapImage**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) está conectado à árvore XAML ativa após a configuração do conteúdo com [**SetSourceAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) ou [**UriSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.urisource).
-   A imagem é decodificada usando decodificação assíncrona, como [**SetSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsource).
-   A imagem é oculta definindo [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) como 0 ou [**Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) como **Collapsed** no elemento de imagem host, no pincel ou em qualquer elemento pai.
-   O controle de imagem ou pincel usa um [**Stretch**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch) de **None**.
-   A imagem é usada como um [**NineGrid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.ninegrid).
-   `CacheMode="BitmapCache"` é configurado no elemento de imagem ou em qualquer elemento pai.
-   O pincel de imagem é não retangular (como quando aplicado a uma forma ou um texto).

Nas situações acima, definir um tamanho de decodificação explícito é a única maneira de conseguir economia de memória.

Você sempre deve anexar uma [**BitmapImage**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) à árvore ativa antes de definir a origem. Sempre que um elemento de imagem ou um pincel for especificado na marcação, esse será automaticamente o caso. Exemplos são fornecidos abaixo, sob o título "Exemplos de árvore ativa". Você sempre deve evitar usar [**SetSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsource) e usar, ao invés, [**SetSourceAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) quando estiver configurando uma fonte de fluxo. E é uma boa ideia evitar ocultar conteúdo de imagem (seja com opacidade zero ou visibilidade colapsada) enquanto espera que o evento [**ImageOpened**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.imageopened) seja acionado. Isso é uma chamada de julgamento: você não se beneficiará com a decodificação de tamanho certo automática se for feita. Se seu aplicativo deve ocultar conteúdo de imagem inicialmente, então, ele também deve definir o tamanho de decodificação explicitamente, se possível.

**Exemplos de árvore dinâmica**

Exemplo 1 (bom) - Uniform Resource Identifier (URI) especificado em marcação.

```xaml
<Image x:Name="myImage" UriSource="Assets/cool-image.png"/>
```

Exemplo 2 marcação - URI especificado em code-behind.

```xaml
<Image x:Name="myImage"/>
```

Exemplo 2 code-behind (bom) - conectando o BitmapImage à arvore antes de definir o UriSource.

```csharp
var bitmapImage = new BitmapImage();
myImage.Source = bitmapImage;
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
```

Exemplo 2 de code-behind (ruim) – configurar o UriSource do BitmapImage antes de conectá-lo à arvore.

```csharp
var bitmapImage = new BitmapImage();
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
myImage.Source = bitmapImage;
```

### <a name="caching-optimizations"></a>Otimizações em cache

As otimizações em cache servem, efetivamente, para imagens que usam [**UriSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.urisource) para carregar conteúdo de um pacote do aplicativo ou a partir da Web. O URI é usado para identificar exclusivamente o conteúdo subjacente e, internamente, a estrutura XAML não vai baixar ou decodificar o conteúdo várias vezes. Em vez disso, usará os recursos de hardware ou software em cache para exibir o conteúdo várias vezes.

A exceção a essa otimização é quando a imagem é exibida várias vezes em diferentes resoluções (que podem ser especificadas explicitamente ou por meio de decodificação automática de tamanho correto). Cada entrada do cache também armazena a resolução da imagem e, se o XAML não encontrar uma imagem com um URI de origem que corresponda à resolução necessária, ele decodificará uma nova versão com esse tamanho. Contudo, ele não vai baixar os dados de imagem codificados novamente.

Consequentemente, você deve adotar o uso de [**UriSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.urisource) quando estiver carregando imagens de um pacote do aplicativo e evitar usar um fluxo de arquivo e [**SetSourceAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) quando não for exigido.

### <a name="images-in-virtualized-panels-listview-for-instance"></a>Imagens em painéis virtualizados (ListView, por exemplo)

Se uma imagem for removida da árvore, porque o aplicativo a removeu explicitamente ou porque ela está em um painel virtualizado moderno e foi removida implicitamente quando rolada para fora da vista, o XAML otimizará o uso de memória liberando os recursos de hardware para a imagem, já que não são mais exigidos. A memória não é liberada imediatamente, mas, em vez disso, é liberada durante a atualização de quadro que ocorre um segundo após o elemento de imagem não estar mais na árvore.

Consequentemente, você deve se esforçar para usar painéis virtualizados modernos para hospedar listas de conteúdo de imagem.

### <a name="software-rasterized-images"></a>Imagens rasterizadas de software

Quando uma imagem é usada para um pincel não retangular ou para um [**NineGrid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.ninegrid), ela usará um caminho de rasterização de software, que não dimensionará imagens de jeito nenhum. Além disso, ele deve armazenar uma cópia da imagem na memória de hardware e software. Por exemplo, se uma imagem é usada como um pincel para uma elipse, então a imagem inteira potencialmente grande será armazenada duas vezes internamente. Quando for usar um **NineGrid** ou um pincel não retangular, seu aplicativo deve pré-dimensionar as imagens para aproximadamente o tamanho em que elas serão renderizadas.

### <a name="background-thread-image-loading"></a>Carregamento de imagem do thread em segundo plano

O XAML tem uma otimização interna que permite que ele decodifique o conteúdo de uma imagem de forma assíncrona para uma superfície na memória de hardware, sem a necessidade de uma superfície intermediária na memória de software. Isso reduz o pico de uso da memória e a latência de renderização. Esse recurso será desativado se qualquer uma das seguintes condições for atendida.

-   A imagem é usada como um [**NineGrid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.ninegrid).
-   `CacheMode="BitmapCache"` é configurado no elemento de imagem ou em qualquer elemento pai.
-   O pincel de imagem é não retangular (como quando aplicado a uma forma ou um texto).

### <a name="softwarebitmapsource"></a>SoftwareBitmapSource

A classe [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) troca imagens descomprimidas interoperáveis entre diferentes namespaces WinRT, como [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder), APIs de câmera e XAML. Essa classe evita uma cópia extra que, tipicamente, seria necessária com [**WriteableBitmap**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap), e isso ajuda a reduzir memória de pico e latência origem para tela.

O [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) que fornece informações de origem também pode ser configurado para usar um [**IWICBitmap**](https://docs.microsoft.com/windows/desktop/api/wincodec/nn-wincodec-iwicbitmap) personalizado para fornecer um armazenamento de apoio recarregável que permite que o aplicativo remapeie a memória como ele achar adequado. Isso é um caso de uso de C++ avançado.

Seu aplicativo deve usar [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) e [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) para interoperar com outras APIs WinRT que produzem e consomem imagens. E seu aplicativo deve usar **SoftwareBitmapSource** quando estiver carregando dados de imagem descomprimidos, em vez de usar [**WriteableBitmap**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap).

### <a name="use-getthumbnailasync-for-thumbnails"></a>Use GetThumbnailAsync para miniaturas

Um caso de uso para o dimensionamento de imagens é a criação de miniaturas. Embora você possa usar [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) e [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight) para providenciar versões menores de imagens, a UWP fornece APIs ainda mais eficientes para a recuperação de miniaturas. [**GetThumbnailAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getthumbnailasync) fornece as miniaturas para imagens que já têm o sistema de arquivo em cache. Isso oferece um desempenho ainda melhor do que as APIs do XAML porque a imagem não precisa ser aberta ou decodificada.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> FileOpenPicker picker = new FileOpenPicker();
> picker.FileTypeFilter.Add(".bmp");
> picker.FileTypeFilter.Add(".jpg");
> picker.FileTypeFilter.Add(".jpeg");
> picker.FileTypeFilter.Add(".png");
> picker.SuggestedStartLocation = PickerLocationId.PicturesLibrary;
>
> StorageFile file = await picker.PickSingleFileAsync();
>
> StorageItemThumbnail fileThumbnail = await file.GetThumbnailAsync(ThumbnailMode.SingleItem, 64);
>
> BitmapImage bmp = new BitmapImage();
> bmp.SetSource(fileThumbnail);
>
> Image img = new Image();
> img.Source = bmp;
> ```
> ```vb
> Dim picker As New FileOpenPicker()
> picker.FileTypeFilter.Add(".bmp")
> picker.FileTypeFilter.Add(".jpg")
> picker.FileTypeFilter.Add(".jpeg")
> picker.FileTypeFilter.Add(".png")
> picker.SuggestedStartLocation = PickerLocationId.PicturesLibrary
>
> Dim file As StorageFile = Await picker.PickSingleFileAsync()
>
> Dim fileThumbnail As StorageItemThumbnail = Await file.GetThumbnailAsync(ThumbnailMode.SingleItem, 64)
>
> Dim bmp As New BitmapImage()
> bmp.SetSource(fileThumbnail)
>
> Dim img As New Image()
> img.Source = bmp
> ```

### <a name="decode-images-once"></a>Decodificar imagens uma vez

Para evitar que as imagens sejam decodificadas mais de uma vez, assine a propriedade [**Image.Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) de um URI, em vez de usar fluxos de memória. Essa estrutura XAML pode associar o mesmo URI em vários locais com uma imagem decodificada, mas não pode fazer o mesmo para vários fluxos de memória que contenham os mesmos dados e cria uma imagem decodificada diferente para cada fluxo de memória.
