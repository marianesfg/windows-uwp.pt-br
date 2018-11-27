---
Description: Use cross-slide to support selection with the swipe gesture and drag (move) interactions with the slide gesture.
title: Diretrizes de deslizamento transversal
ms.assetid: 897555e2-c567-4bbe-b600-553daeb223d5
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f6a37af19c9e3e9beaebbadfcc71f3ad01e3087c
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7711056"
---
# <a name="guidelines-for-cross-slide"></a>Diretrizes de deslizamento transversal




**APIs importantes**

-   [**CrossSliding**](https://msdn.microsoft.com/library/windows/apps/br241942)
-   [**CrossSlideThresholds**](https://msdn.microsoft.com/library/windows/apps/br241941)
-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)

Use o deslizamento transversal para dar suporte à seleção com o gesto de deslizar e a interações de arrastar (mover) com o gesto de deslizar.

## <a name="span-iddosanddontsspanspan-iddosanddontsspanspan-iddosanddontsspandos-and-donts"></a><span id="Dos_and_don_ts"></span><span id="dos_and_don_ts"></span><span id="DOS_AND_DON_TS"></span>O que fazer e o que não fazer


-   Use o deslizamento transversal para listas ou coleções que rolam em uma única direção.
-   Utilize o deslizamento transversal para seleção de itens quando a interação de toque for utilizada para outra finalidade.
-   Não utilize o deslizamento transversal para adicionar itens a uma fila.

## <a name="span-idadditionalusageguidancespanspan-idadditionalusageguidancespanspan-idadditionalusageguidancespanadditional-usage-guidance"></a><span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>Diretrizes de uso adicional


Gestos de selecionar e arrastar só são possíveis em uma área de conteúdo que permite movimento panorâmico unidirecional (vertical ou horizontal). Para qualquer uma dessas interações funcionar, uma direção de movimento panorâmico deve ser bloqueada, e o gesto deve ser realizado na direção perpendicular à direção do momento panorâmico.

Veja aqui a demonstração de selecionar e arrastar um objeto usando o deslizamento transversal. A imagem à esquerda mostra como um item é selecionado quando um gesto de passar o dedo não atravessa o limite de distância antes que o contato seja retirado e o objeto, liberado. A imagem à direita mostra um gesto de deslizamento que atravessa o limite de distância e faz com que o objeto seja arrastado.

![diagrama mostrando os processos de selecionar e de arrastar e soltar.](images/crossslide-mechanism.png)

As distâncias de limite usadas pela interação de deslizamento transversal são mostradas no diagrama a seguir.

![captura de tela mostrando os processos de selecionar e de arrastar e soltar.](images/crossslide-threshold.png)

Para preservar a funcionalidade de movimento panorâmico, um pequeno limite de 2,7 mm (aproximadamente 10 pixels na resolução de destino) deve ser excedido para que uma interação de selecionar ou arrastar seja ativada. Esse pequeno limite ajuda o sistema a diferenciar entre deslizamento transversal e movimento panorâmico. Ajuda também a garantir que um gesto de toque seja distinguido do deslizamento transversal e do movimento panorâmico.

Esta imagem mostra como um usuário toca um elemento da interface do usuário, mas move o dedo ligeiramente para baixo no contato. Não havendo limites, a interação é interpretada como um deslizamento transversal devido ao movimento vertical inicial. Com o limite, o movimento é interpretado corretamente como movimento panorâmico horizontal.

![captura de tela mostrando o limite que remove a ambiguidade das interações de seleção e de arrastar e soltar.](images/crossslide-threshold2.png)

Aqui estão algumas diretrizes a serem consideradas ao incluir a funcionalidade de deslizamento transversal em seu aplicativo.

Use o deslizamento transversal para listas ou coleções que rolam em uma única direção. Para obter mais informações, consulte [Adicionando controles ListView](https://msdn.microsoft.com/library/windows/apps/hh465382).

**Observação**em casos onde a área de conteúdo pode ter movimento panorâmico nas duas direções, como navegadores da web ou leitores eletrônicos, a interação com o pressionar e manter pressionado deve ser usada para invocar o menu de contexto para objetos como imagens e hiperlinks.

 

|                                                                                         |                                                                                         |
|-----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| ![movimento panorâmico horizontal, lista bidimensional](images/groupedlistview1.png)                | ![movimento panorâmico vertical, lista unidimensional](images/listviewlistlayout.png)                |
| Uma lista bidimensional de movimento panorâmico horizontal. Arraste verticalmente para selecionar ou mover um item. | Uma lista unidimensional de movimento panorâmico vertical. Arraste verticalmente para selecionar ou mover um item. |

 

### <span id="selection"></span><span id="SELECTION"></span>

**Selecionando**

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

## <a name="span-idrelatedtopicsspanrelated-articles"></a><span id="related_topics"></span>Artigos relacionados


**Exemplos**
* [Amostra de entrada básica](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Amostra de entrada de baixa latência](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Exemplo do modo de interação do usuário](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Exemplo de elementos visuais de foco](http://go.microsoft.com/fwlink/p/?LinkID=619895)
**Exemplos do arquivo**
* [Entrada: amostra de eventos de entrada do usuário XAML](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrada: exemplo de funcionalidades do dispositivo](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: amostra de teste de toque](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [Amostra de rolagem, movimento panorâmico e aplicação de zoom em XAML](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: amostra de tinta simplificada](http://go.microsoft.com/fwlink/p/?linkid=246570)
* [Entrada: amostra de gestos no Windows 8](http://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Entrada: amostra de manipulações e gestos (C++)](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [Amostra de entrada por toque do DirectX](http://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




