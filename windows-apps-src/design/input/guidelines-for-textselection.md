---
Description: Este tópico descreve a nova interface do usuário do Windows para selecionar e manipular texto, imagens e controles e fornece diretrizes de experiência do usuário que devem ser consideradas ao usar esses novos mecanismos de seleção e manipulação em seu aplicativo UWP.
title: Selecionando texto e imagens
ms.assetid: d973ffd8-602e-47b5-ab0b-4b2a964ec53d
label: Selecting text and images
template: detail.hbs
keywords: teclado, texto, entrada, interações do usuário
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 56f09f2c903354159c63fa7226007cd65e57e69e
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257923"
---
# <a name="selecting-text-and-images"></a>Selecionando texto e imagens


Este artigo descreve a seleção e a manipulação de texto, imagens e controles e fornece diretrizes da experiência do usuário que devem ser consideradas ao usar esses mecanismos em seus apps.

> **APIs importantes**: [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)
 


## <a name="dos-and-donts"></a>O que fazer e o que não fazer


-   Use glifos de fonte ao implementar sua interface do usuário de garra. A garra é uma combinação das duas fontes Segoe da interface do usuário disponíveis em todo o sistema. O uso de recursos de fonte simplifica problemas de renderização em diferentes dpis e funciona bem com os vários níveis de escala da interface do usuário. Ao implementar suas próprias garras, elas devem compartilhar as seguintes características da interface do usuário:

    -   Forma circular
    -   Visível em qualquer tela de fundo
    -   Tamanho uniforme
-   Forneça uma margem ao redor do conteúdo selecionável para acomodar a interface do usuário da garra. Se o aplicativo permitir a seleção de texto em uma região que não ofereça movimento panorâmico/rolagem, deixe uma margem de 1/2 garra nos lados esquerdo e direito da área de texto e uma altura de 1 garra nos lados superior e inferior da área de texto (como mostram as imagens a seguir). Isso garante que toda a interface do usuário da garra seja exposta ao usuário e minimiza interações não pretendidas com outras partes de interface do usuário baseadas em borda.

    ![margens da garra de seleção de texto](images/textselection-gripper-margins.png)

-   Oculte a interface do usuário das garras durante a interação. Elimina a obstrução causada pelas garras durante a interação. Isso é útil quando uma garra não está totalmente oculta pelo dedo ou há várias garras de seleção de texto. Isso elimina artefatos visuais ao exibir janelas filho.

-   Não permita a seleção de elementos da interface do usuário como controles, rótulos, imagens, conteúdo proprietário e assim por diante. Normalmente, os aplicativos do Windows permitem a seleção somente dentro de controles específicos. Controles, como botões, rótulos e logotipos não são selecionáveis. Avalie se a seleção é um problema para o seu aplicativo e, se for, identifique as áreas da interface do usuário onde a seleção deve ser proibida. 

## <a name="additional-usage-guidance"></a>Diretriz de uso adicional


A seleção e a manipulação de texto são particularmente suscetíveis aos desafios da experiência do usuário introduzidos pelas interações por toque. A entrada por mouse, caneta/stylus e teclado são altamente granulares: um clique do mouse ou contato da caneta/stylus é mapeado normalmente para um único pixel e uma tecla é ou não é pressionada. A entrada de toque não é granular; é difícil mapear toda a superfície da ponta de um dedo para especificar a localização x-y na tela para colocar o sinal de interpolação de texto de forma precisa.

**Considerações e recomendações**

Use os controles internos expostos por meio das estruturas de linguagem no Windows para criar aplicativos que fornecem a experiência de interação do usuário de plataforma completa, incluindo comportamentos de seleção e manipulação. Você achará a funcionalidade de interação dos controles incorporados suficiente para a maioria dos aplicativos UWP.

Ao usar controles de texto da UWP padrão, os comportamentos de seleção e os recursos visuais descritos neste tópico não podem ser personalizados.

**Seleção de texto**

Se seu aplicativo exigir uma interface do usuário personalizada que dê suporte à seleção de texto, recomendamos que você siga os comportamentos de seleção do Windows descritos aqui.

**Conteúdo editável e não editável**


Com o toque, as interações de seleção são realizadas principalmente pelos gestos, como um toque para definir um cursor de inserção ou selecionar uma palavra e um movimento de deslizar para modificar a seleção. Assim como acontece com outras interações com o Windows Touch, as interações temporais são limitadas ao gesto de pressionar e manter para exibir a interface do usuário informativa. Para obter mais informações, consulte [Diretrizes de resposta visual](guidelines-for-visualfeedback.md).

O Windows reconhece dois Estados possíveis para interações de seleção, editável e não editável, e ajusta a interface de usuário de seleção, os comentários e a funcionalidade de acordo.

**Conteúdo editável**

Tocar na metade esquerda de uma palavra desloca o cursor para a esquerda imediata da palavra, enquanto tocar na metade direita desloca o cursor para a direita imediata da palavra.

A imagem a seguir demonstra como colocar um cursor de inserção inicial com uma garra ao tocar próximo ao início ou ao final de uma palavra.

![tocar (ou pressionar e segurar) o lado esquerdo de uma palavra para colocar um sinal de interpolação e uma garra no início dessa palavra. tocar (ou pressionar e segurar) o lado direito de uma palavra para colocar um sinal de interpolação e uma garra no final dessa palavra.](images/textselection-place-caret.png)

A imagem a seguir demonstra como ajustar uma seleção ao arrastar a garra.

![arraste a garra em ambas as direções para ajustar a seleção (a primeira garra permanece ancorada e a segunda é exibida). arraste qualquer uma das garras para fazer ajustes subsequentes.](images/adjust-selection.png)

As imagens a seguir demonstram como invocar o menu de contexto tocando na seleção ou em uma garra (a ação de pressionar e segurar também pode ser usada).

![tocar (ou pressionar e segurar) dentro da seleção ou em uma garra para invocar o menu de contexto.](images/textselection-show-context.png)

**Observação**  essas interações variam um pouco no caso de uma palavra incorreta. Tocar uma palavra que está marcada com a ortografia errada ressaltará toda a palavra e invocará o menu de contexto com a sugestão ortográfica.

 

**Conteúdo não editável**

A imagem a seguir demonstra como selecionar uma palavra tocando na palavra (espaços não são incluídos na seleção inicial).

![tocar em uma palavra para selecioná-la (espaços não são incluídos na seleção inicial).](images/select-word.png)

Siga os mesmos procedimentos para textos editáveis para ajustar a seleção e exibir o menu de contexto.

**Manipulação de objetos**

Sempre que possível, use os mesmos recursos de garra (ou semelhantes) da seleção de texto ao implementar manipulação personalizada de objetos em um aplicativo UWP. Isso ajuda a proporcionar uma experiência de interação consistente na plataforma.

Por exemplo, as garras também podem ser usadas em aplicativos de processamento de imagens que dão suporte a redimensionamento e corte ou aplicativos de media player que fornecem barras de progresso ajustáveis, conforme mostram as imagens a seguir.

![media player com garra de progresso](images/gripper-mediaplayer.png)

*Player de mídia com barra de progresso ajustável.*

![Imagem com garras de corte](images/gripper-imagemanip.png)

*Editor de imagem com garras de corte.*

## <a name="related-articles"></a>Artigos relacionados



**Para desenvolvedores**
* [Interações personalizadas do usuário](https://docs.microsoft.com/windows/uwp/design/layout/index)

**Exemplos**
* [Amostra de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [Exemplo de entrada de baixa latência](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [Amostra do modo de interação do usuário](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
* [Amostra de elementos visuais de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

**Amostras de arquivo-morto**
* [Entrada: exemplo de eventos de entrada do usuário XAML](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
* [Entrada: exemplo de recursos do dispositivo](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
* [Entrada: exemplo de teste de colisão de toque](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
* [Exemplo de rolagem, panorâmica e zoom do XAML](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
* [Entrada: exemplo de tinta simplificada](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
* [Entrada: exemplo de gestos do Windows 8](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
* [Entrada: exemplo de manipulações e gestos (C++)](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
* [Exemplo de entrada do DirectX Touch](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
 

 




