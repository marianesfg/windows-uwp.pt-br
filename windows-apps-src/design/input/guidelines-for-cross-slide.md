---
Description: Use o deslizamento transversal para dar suporte à seleção com o gesto de deslizar e a interações de arrastar (mover) com o gesto de deslizar.
title: Diretrizes de deslizamento transversal
ms.assetid: 897555e2-c567-4bbe-b600-553daeb223d5
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 685c71ca0e6ed0989932b7c9a0169088d5b83bd2
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82558833"
---
# <a name="guidelines-for-cross-slide"></a>Diretrizes de deslizamento transversal




**APIs importantes**

-   [**CrossSliding**](https://docs.microsoft.com/uwp/api/windows.ui.input.gesturerecognizer.crosssliding)
-   [**CrossSlideThresholds**](https://docs.microsoft.com/uwp/api/windows.ui.input.gesturerecognizer.crossslidethresholds)
-   [**Windows. UI. Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)

Use o deslizamento transversal para dar suporte à seleção com o gesto de deslizar e a interações de arrastar (mover) com o gesto de deslizar.

## <a name="span-iddos_and_don_tsspanspan-iddos_and_don_tsspanspan-iddos_and_don_tsspandos-and-donts"></a><span id="Dos_and_don_ts"></span><span id="dos_and_don_ts"></span><span id="DOS_AND_DON_TS"></span>Dos e não


-   Use o deslizamento transversal para listas ou coleções que rolam em uma única direção.
-   Utilize o deslizamento transversal para seleção de itens quando a interação de toque for utilizada para outra finalidade.
-   Não utilize o deslizamento transversal para adicionar itens a uma fila.

## <a name="span-idadditional_usage_guidancespanspan-idadditional_usage_guidancespanspan-idadditional_usage_guidancespanadditional-usage-guidance"></a><span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>Diretrizes de uso adicionais


Gestos de selecionar e arrastar só são possíveis em uma área de conteúdo que permite movimento panorâmico unidirecional (vertical ou horizontal). Para qualquer uma dessas interações funcionar, uma direção de movimento panorâmico deve ser bloqueada, e o gesto deve ser realizado na direção perpendicular à direção do momento panorâmico.

Veja aqui a demonstração de selecionar e arrastar um objeto usando o deslizamento transversal. A imagem à esquerda mostra como um item é selecionado quando um gesto de passar o dedo não atravessa o limite de distância antes que o contato seja retirado e o objeto, liberado. A imagem à direita mostra um gesto de deslizamento que atravessa o limite de distância e faz com que o objeto seja arrastado.

![diagrama mostrando os processos de selecionar e de arrastar e soltar.](images/crossslide-mechanism.png)

As distâncias de limite usadas pela interação de deslizamento transversal são mostradas no diagrama a seguir.

![captura de tela mostrando os processos de selecionar e de arrastar e soltar.](images/crossslide-threshold.png)

Para preservar a funcionalidade de movimento panorâmico, um pequeno limite de 2,7 mm (aproximadamente 10 pixels na resolução de destino) deve ser excedido para que uma interação de selecionar ou arrastar seja ativada. Esse pequeno limite ajuda o sistema a diferenciar entre deslizamento transversal e movimento panorâmico. Ajuda também a garantir que um gesto de toque seja distinguido do deslizamento transversal e do movimento panorâmico.

Esta imagem mostra como um usuário toca um elemento da interface do usuário, mas move o dedo ligeiramente para baixo no contato. Não havendo limites, a interação é interpretada como um deslizamento transversal devido ao movimento vertical inicial. Com o limite, o movimento é interpretado corretamente como movimento panorâmico horizontal.

![captura de tela mostrando o limite que remove a ambiguidade das interações de seleção e de arrastar e soltar.](images/crossslide-threshold2.png)

Aqui estão algumas diretrizes a serem consideradas ao incluir a funcionalidade de deslizamento transversal em seu aplicativo.

Use o deslizamento transversal para listas ou coleções que rolam em uma única direção. Para obter mais informações, consulte [Adicionando controles ListView](https://docs.microsoft.com/previous-versions/windows/apps/hh465382(v=win.10)).

**Observação**    Nos casos em que a área de conteúdo pode ser panorâmica em duas direções, como navegadores da Web ou leitores eletrônicos, a interação com tempo de pressionar e manter deve ser usada para invocar o menu de contexto para objetos como imagens e hiperlinks.

 

|                                                                                         |                                                                                         |
|-----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| ![movimento panorâmico horizontal, lista bidimensional](images/groupedlistview1.png)                | ![movimento panorâmico vertical, lista unidimensional](images/listviewlistlayout.png)                |
| Uma lista bidimensional de movimento panorâmico horizontal. Arraste verticalmente para selecionar ou mover um item. | Uma lista unidimensional de movimento panorâmico vertical. Arraste verticalmente para selecionar ou mover um item. |

 

### <span id="selection"></span><span id="SELECTION"></span>

**Seleção**

Seleção é marcar, sem iniciar ou ativar, um ou mais objetos. Esta ação é semelhante a dar um único clique no mouse, ou pressionar a tecla Shift e clicar com o mouse, em um ou mais objetos.

A seleção de deslizamento transversal é atingida tocando em um elemento e liberando-o após uma curta interação de arrastar. Esse método de seleção usa tanto o modo de seleção dedicado quanto a interação com tempo de pressionar e manter pressionado exigida por outras interfaces de toque e não entra em conflito com a interação de toque para ativação.

Além do limite de distância, a seleção de deslizamento transversal é limitada a uma área de limite de 90°, conforme mostrado no diagrama a seguir. Se o objeto for arrastado para fora dessa área, ele não será selecionado.

![diagrama mostrando a área de limite de seleção.](images/crossslide-selection.png)

A interação de deslizamento transversal é complementada por uma interação do tempo de pressionar e manter pressionado, também chamada de interação "autorreveladora". Essa interação complementar ativa uma animação que indica qual ação poderá ser executada no objeto. Para saber mais sobre a UI de desambiguidade, veja [Diretrizes de resposta visual](guidelines-for-visualfeedback.md).

As capturas de tela a seguir demonstram como a animação autorreveladora funciona.

1.  Pressione e mantenha pressionada a animação da interação autorreveladora. O estado selecionado do item afeta o que é revelado pela animação: uma marca de verificação se não selecionado e nenhuma marca de verificação se selecionado.

    ![captura de tela mostrando um estado não selecionado.](images/crossslide-selfreveal1.png)

2.  Selecione o item usando o gesto de deslizar (para cima ou para baixo).

    ![captura de tela mostrando a animação da seleção.](images/crossslide-selfreveal2.png)

3.  O item não é selecionado. Substitua o comportamento da seleção usando o gesto de deslizar para mover o item.

    ![captura de tela mostrando a animação de arrastar e soltar.](images/crossslide-selfreveal3.png)

Use um único toque para seleção em aplicativos em que esta é a única ação primária. A animação autorreveladora de deslizamento transversal é exibida para remover a ambiguidade dessa funcionalidade da interação por toque padrão para ativação e navegação.

**Cesta de seleção**

A cesta de seleção é uma representação visualmente distinta e dinâmica de itens que foram selecionados na lista principal ou coleção no aplicativo. Esse recurso é útil para acompanhar os itens selecionados e deve ser usado pelo aplicativos quando:

-   Os itens podem ser selecionados de vários locais.
-   Muitos itens podem ser selecionados.
-   Uma ação ou um comando depende da lista de seleção.

O conteúdo da cesta de seleção persiste entre ações e comandos. Por exemplo, se você selecionar uma série de fotografias em uma galeria, aplicar uma correção de cor a cada fotografia e compartilhar as fotografias de alguma forma, os itens permanecerão selecionados.

Se nenhuma cesta de seleção for utilizada em um aplicativo, a seleção atual deve ser limpa após uma ação ou comando. Por exemplo, se você selecionar uma música em uma lista de reprodução e classificá-la, a seleção deve ser limpa.

A seleção atual também deve ser limpa quando nenhuma cesta de seleção for usada e outro item na lista ou coleção for ativado. Por exemplo, se você selecionar uma mensagem da caixa de entrada, o painel de visualização será atualizado. Então, se você selecionar uma segunda mensagem da caixa de entrada, a seleção da mensagem anterior será cancelada e o painel de visualização será atualizado.

**Filas**

Uma fila não é equivalente à lista da cesta de seleção e não deve ser tratada como tal. As principais diferenças são:

-   A lista de itens na cesta de seleção é apenas uma representação visual; os itens em uma fila são montados com uma ação específica em mente.
-   É possível representar os itens apenas uma vez na cesta de seleção, mas várias vezes na fila.
-   A ordem dos itens na cesta de seleção representa a ordem de seleção. A ordem dos itens na fila está diretamente relacionadas à funcionalidade.

Por esses motivos, a interação de seleção de deslizamento transversal não deve ser usada para adicionar itens a uma fila. Em vez disso, os itens devem ser adicionados a uma fila por meio de uma ação de arrastar.

### <span id="draganddrop"></span><span id="DRAGANDDROP"></span>

**Arrastar**

Use o gesto de arrastar para mover um ou mais objetos de um local para outro.

Se for necessário mover mais de um objeto, permita que os usuários selecionem vários itens e depois arrastem todos eles de uma só vez.

## <a name="related-articles"></a>Artigos relacionados

### <a name="samples"></a>Exemplos

- [Amostra de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Amostra de entrada de baixa latência](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Amostra do modo de interação do usuário](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Amostra de visuais de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Exemplos de arquivo-morto

- [Entrada: amostra de eventos de entrada do usuário XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Entrada: amostra de funcionalidades do dispositivo](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrada: amostra de teste de hit de toque](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [Exemplo de rolagem, panorâmica e zoom do XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Entrada: amostra de tinta simplificada](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [Entrada: amostra de gestos no Windows 8](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
- [Entrada: exemplo de manipulações e gestos](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Amostra de entrada por toque do DirectX](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
 

 




