---
Description: Este tópico descreve a nova interface do Windows para selecionar e manipular texto, imagens e controles e fornece diretrizes de experiência do usuário que devem ser consideradas ao usar esses mecanismos de manipulação e nova seleção no seu aplicativo da UWP.
title: Selecionando texto e imagens
ms.assetid: d973ffd8-602e-47b5-ab0b-4b2a964ec53d
label: Selecting text and images
template: detail.hbs
keywords: teclado, texto, entrada, interações do usuário
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fc227904e57577a30eb2d171a060fb69d39b71f0
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363494"
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

Use os controles internos expostos por meio de estruturas de linguagem no Windows para criar aplicativos que fornecem a experiência de interação de usuário de plataforma completo, incluindo comportamentos de seleção e a manipulação. Você achará a funcionalidade de interação dos controles incorporados suficiente para a maioria dos aplicativos UWP.

Ao usar controles de texto da UWP padrão, os comportamentos de seleção e os recursos visuais descritos neste tópico não podem ser personalizados.

**Seleção de texto**

Se seu aplicativo requer uma interface do usuário personalizada que dá suporte à seleção de texto, é recomendável que você siga os comportamentos de seleção do Windows descritos aqui.

**Conteúdo editável e não editáveis**


Com o toque, as interações de seleção são realizadas principalmente pelos gestos, como um toque para definir um cursor de inserção ou selecionar uma palavra e um movimento de deslizar para modificar a seleção. Como com outros Windows touch interações, interações cronometradas estão limitadas à imprensa e mantenha o gesto para exibir a informação da interface do usuário. Para obter mais informações, consulte [Diretrizes de resposta visual](guidelines-for-visualfeedback.md).

Windows reconhece dois estados possíveis para interações de seleção, editáveis e não editáveis e também se ajusta a seleção da interface do usuário, comentários e funcionalidade.

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

*Player de mídia com a barra de progresso ajustáveis.*

![Imagem com garras de corte](images/gripper-imagemanip.png)

*Editor de imagens com as garras de corte.*

## <a name="related-articles"></a>Artigos relacionados



**Para desenvolvedores**
* [Interações personalizadas do usuário](https://developer.microsoft.com/windows/design/inputs-devices)

**Exemplos**
* [Exemplo de entrada básico](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Exemplo de entrada de baixa latência](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Amostra do modo de interação do usuário](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Amostra de elementos visuais de foco](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**Amostras de arquivo-morto**
* [Entrada: Exemplo de eventos de entrada do usuário XAML](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrada: Exemplo de recursos do dispositivo](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: Exemplo de teste de hit de toque](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML de rolagem, movimento panorâmico e zoom de exemplo](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: Exemplo simplificado de tinta](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [Entrada: Exemplo de gestos do Windows 8](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Entrada: Manipulações e exemplo de gestos (C++)](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [Exemplo de entrada de toque do DirectX](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




