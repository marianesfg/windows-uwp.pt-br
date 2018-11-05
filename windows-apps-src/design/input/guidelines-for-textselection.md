---
author: Karl-Bridge-Microsoft
Description: This topic describes the new Windows UI for selecting and manipulating text, images, and controls and provides user experience guidelines that should be considered when using these new selection and manipulation mechanisms in your UWP app.
title: Selecionando texto e imagens
ms.assetid: d973ffd8-602e-47b5-ab0b-4b2a964ec53d
label: Selecting text and images
template: detail.hbs
keywords: teclado, texto, entrada, interações do usuário
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 32d8e7d858ff28a6ef6a9af517e0a584c6106cd5
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6039050"
---
# <a name="selecting-text-and-images"></a>Selecionando texto e imagens


Este artigo descreve a seleção e a manipulação de texto, imagens e controles e fornece diretrizes da experiência do usuário que devem ser consideradas ao usar esses mecanismos em seus apps.

> **APIs importantes**: [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994), [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)
 


## <a name="dos-and-donts"></a>O que fazer e o que não fazer


-   Use glifos de fonte ao implementar sua interface do usuário de garra. A garra é uma combinação das duas fontes Segoe da interface do usuário disponíveis em todo o sistema. O uso de recursos de fonte simplifica problemas de renderização em diferentes dpis e funciona bem com os vários níveis de escala da interface do usuário. Ao implementar suas próprias garras, elas devem compartilhar as seguintes características da interface do usuário:

    -   Forma circular
    -   Visível em qualquer tela de fundo
    -   Tamanho uniforme
-   Forneça uma margem ao redor do conteúdo selecionável para acomodar a interface do usuário da garra. Se o aplicativo permitir a seleção de texto em uma região que não ofereça movimento panorâmico/rolagem, deixe uma margem de 1/2 garra nos lados esquerdo e direito da área de texto e uma altura de 1 garra nos lados superior e inferior da área de texto (como mostram as imagens a seguir). Isso garante que toda a interface do usuário da garra seja exposta ao usuário e minimiza interações não pretendidas com outras partes de interface do usuário baseadas em borda.

    ![margens da garra de seleção de texto](images/textselection-gripper-margins.png)

-   Oculte a interface do usuário das garras durante a interação. Elimina a obstrução causada pelas garras durante a interação. Isso é útil quando uma garra não está totalmente oculta pelo dedo ou há várias garras de seleção de texto. Isso elimina artefatos visuais ao exibir janelas filho.

-   Não permita a seleção de elementos da interface do usuário como controles, rótulos, imagens, conteúdo proprietário e assim por diante. Normalmente, os aplicativos do Windows permitem a seleção somente dentro de controles específicos. Controles, como botões, rótulos e logotipos não são selecionáveis. Avalie se a seleção é um problema para o seu aplicativo e, se for, identifique as áreas da interface do usuário onde a seleção deve ser proibida. 

## <a name="additional-usage-guidance"></a>Diretrizes de uso adicionais


A seleção e a manipulação de texto são particularmente suscetíveis aos desafios da experiência do usuário introduzidos pelas interações por toque. A entrada por mouse, caneta/stylus e teclado são altamente granulares: um clique do mouse ou contato da caneta/stylus é mapeado normalmente para um único pixel e uma tecla é ou não é pressionada. A entrada de toque não é granular; é difícil mapear toda a superfície da ponta de um dedo para especificar a localização x-y na tela para colocar o sinal de interpolação de texto de forma precisa.

**Considerações e recomendações**

Use os controles internos expostos por meio das estruturas de idioma em aplicativos de compilação Windowsto que proporcionam a experiência de interação do usuário plataforma inteira, incluindo comportamentos de seleção e manipulação. Você achará a funcionalidade de interação dos controles incorporados suficiente para a maioria dos aplicativos UWP.

Ao usar controles de texto da UWP padrão, os comportamentos de seleção e os recursos visuais descritos neste tópico não podem ser personalizados.

**Seleção de texto**

Se seu aplicativo requer uma interface do usuário personalizada que dá suporte à seleção de texto, recomendamos que você siga os comportamentos de Windowsselection descritos aqui.

**Conteúdo editável e não editável**


Com o toque, as interações de seleção são realizadas principalmente pelos gestos, como um toque para definir um cursor de inserção ou selecionar uma palavra e um movimento de deslizar para modificar a seleção. Assim como acontece com outras interações Windowstouch, as interações são limitadas à imprensa e mantenha o gesto para exibir a interface do usuário informativa. Para obter mais informações, consulte [Diretrizes de resposta visual](guidelines-for-visualfeedback.md).

Windowsrecognizes dois possíveis estados para interações de seleção, editáveis e não editável e ajusta a seleção da interface do usuário, comentários e funcionalidade adequadamente.

**Conteúdo editável**

Tocar na metade esquerda de uma palavra desloca o cursor para a esquerda imediata da palavra, enquanto tocar na metade direita desloca o cursor para a direita imediata da palavra.

A imagem a seguir demonstra como colocar um cursor de inserção inicial com uma garra ao tocar próximo ao início ou ao final de uma palavra.

![tocar (ou pressionar e segurar) o lado esquerdo de uma palavra para colocar um sinal de interpolação e uma garra no início dessa palavra. tocar (ou pressionar e segurar) o lado direito de uma palavra para colocar um sinal de interpolação e uma garra no final dessa palavra.](images/textselection-place-caret.png)

A imagem a seguir demonstra como ajustar uma seleção ao arrastar a garra.

![arraste a garra em ambas as direções para ajustar a seleção (a primeira garra permanece ancorada e a segunda é exibida). arraste qualquer uma das garras para fazer ajustes subsequentes.](images/adjust-selection.png)

As imagens a seguir demonstram como invocar o menu de contexto tocando na seleção ou em uma garra (a ação de pressionar e segurar também pode ser usada).

![tocar (ou pressionar e segurar) dentro da seleção ou em uma garra para invocar o menu de contexto.](images/textselection-show-context.png)

**Observação**essas interações podem variar no caso de uma palavra incorreta. Tocar uma palavra que está marcada com a ortografia errada ressaltará toda a palavra e invocará o menu de contexto com a sugestão ortográfica.

 

**Conteúdo não editável**

A imagem a seguir demonstra como selecionar uma palavra tocando na palavra (espaços não são incluídos na seleção inicial).

![tocar em uma palavra para selecioná-la (espaços não são incluídos na seleção inicial).](images/select-word.png)

Siga os mesmos procedimentos para textos editáveis para ajustar a seleção e exibir o menu de contexto.

**Manipulação de objetos**

Sempre que possível, use os mesmos recursos de garra (ou semelhantes) da seleção de texto ao implementar manipulação personalizada de objetos em um aplicativo UWP. Isso ajuda a proporcionar uma experiência de interação consistente na plataforma.

Por exemplo, as garras também podem ser usadas em aplicativos de processamento de imagens que dão suporte a redimensionamento e corte ou aplicativos de media player que fornecem barras de progresso ajustáveis, conforme mostram as imagens a seguir.

![media player com garra de progresso](images/gripper-mediaplayer.png)

*Media player com garra de progresso ajustável.*

![Imagem com garras de corte](images/gripper-imagemanip.png)

*Editor de imagens com garras de corte.*

## <a name="related-articles"></a>Artigos relacionados



**Para desenvolvedores**
* [Interações personalizadas do usuário](https://msdn.microsoft.com/library/windows/apps/mt185599)

**Exemplos**
* [Amostra de entrada básica](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Amostra de entrada de baixa latência](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Amostra do modo de interação do usuário](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Amostra de elementos visuais do foco](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**Exemplos de arquivo-morto**
* [Entrada: amostra de eventos de entrada do usuário XAML](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrada: exemplo de funcionalidades do dispositivo](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: amostra de teste de toque](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [Amostra de rolagem, movimento panorâmico e aplicação de zoom em XAML](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: amostra de tinta simplificada](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [Entrada: amostra de gestos no Windows 8](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Entrada: amostra de manipulações e gestos (C++)](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [Amostra de entrada por toque do DirectX](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




