---
Description: Aprenda técnicas de design dinâmico para personalizar seu aplicativo para dispositivos específicos
title: Técnicas de design responsivo
template: detail.hbs
op-migration-status: ready
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f688522ec8970b1e3570610663f5a3e6cae65793
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "68867411"
---
# <a name="responsive-design-techniques"></a>Técnicas de design responsivo

Os aplicativos UWP usam pixels efetivos para garantir que sua interface do usuário será legível e utilizável em todos os dispositivos do Windows. Sendo assim, por que você desejaria personalizar a interface do usuário do seu aplicativo para uma família de dispositivos específica?

- **Para tornar o uso do espaço mais eficiente e reduzir a necessidade de navegar**

    Se você criar um aplicativo para ter uma boa aparência em um dispositivo que tenha uma tela pequena, como um tablet, o aplicativo poderá ser utilizado em um computador com uma tela muito maior, mas provavelmente haverá algum desperdício de espaço. Você pode personalizar o aplicativo para exibir mais conteúdo quando a tela tiver acima de um determinado tamanho. Por exemplo, um aplicativo de compras pode exibir uma categoria de mercadoria de cada vez em um tablet, mas mostrar várias categorias e produtos simultaneamente em um computador ou laptop.

    Ao colocar mais conteúdo na tela, você reduz a quantidade de navegação que o usuário precisa executar.

- **Para tirar proveito dos recursos dos dispositivos**

    Certos dispositivos têm mais probabilidade de ter determinados recursos de dispositivo. Por exemplo, laptops têm probabilidade de ter um sensor de localização e uma câmera, enquanto uma TV pode não ter nenhum dos dois. Seu aplicativo pode detectar quais recursos estão disponíveis e habilitar recursos que os utilizam.

- **Para otimizar para entrada**

    A biblioteca de controles universais funciona com todos os tipos de entrada (toque, caneta, teclado, mouse), mas você ainda pode otimizar para certos tipos de entrada reorganizando seus elementos de interface do usuário. Por exemplo, se você colocar elementos de navegação na parte inferior da tela, será mais fácil para os usuários de telefone acessá-los, mas a maioria dos usuários de computador espera ver os elementos de navegação na parte superior da tela.

Quando você otimiza a interface do usuário de seu aplicativo para larguras de tela específicas, dizemos que você está criando um design responsivo. Aqui estão as seis técnicas de design responsivo que você pode usar para personalizar a interface do usuário do seu aplicativo.

>[!TIP]
> Muitos controles da UWP implementam automaticamente esses comportamentos responsivos. Para criar uma interface de usuário responsiva, recomendamos verificar os [controles da UWP](../controls-and-patterns/index.md).

## <a name="reposition"></a>Reposicionar

Você pode alterar o local e a posição dos elementos de interface do usuário para aproveitar ao máximo o tamanho da janela. Neste exemplo, a janela menor empilha os elementos verticalmente. Quando o aplicativo é usado em uma janela maior, os elementos podem aproveitar a largura da janela maior.

![Reposicionar](images/rsp-design/rspd-reposition2.gif)

Neste design de exemplo para um aplicativo de fotos, o aplicativo de fotos reposiciona seu conteúdo em telas maiores.

## <a name="resize"></a>Redimensionar

Você pode otimizar o tamanho da janela, ajustando as margens e o tamanho dos elementos da interface do usuário. Por exemplo, isso poderia melhorar a experiência de leitura em uma tela maior, simplesmente aumentando o quadro de conteúdo.

![Redimensionar elementos de design](images/rsp-design/rspd-resize2.gif)

## <a name="reflow"></a>Refluxo

Alterando o fluxo de elementos da interface do usuário com base no dispositivo e na orientação, seu aplicativo pode oferecer uma exibição ideal do conteúdo. Por exemplo, ao passar para uma tela maior, talvez faça sentido adicionar colunas, usar contêineres maiores ou gerar itens de lista de maneira diferente.

Este exemplo mostra como uma única coluna de conteúdo de rolagem vertical em uma tela menor pode refluir em uma tela maior para exibir duas colunas de texto.

![Refluir elementos de design](images/rsp-design/rspd_reflow.gif)

## <a name="showhide"></a>Mostrar/ocultar

Você pode mostrar ou ocultar elementos da interface do usuário com base no estado real da tela ou quando o dispositivo é compatível com as funcionalidades adicionais, situações específicas ou orientações de tela preferenciais.

![Ocultar elementos de design](images/rsp-design/rspd-revealhide.gif)

Por exemplo, os controles do player de mídia reduzem o conjunto de botões em telas menores e expandem em telas maiores. O player de mídia em uma janela maior pode lidar com muito mais funcionalidades na tela do que em uma janela menor.

Parte da técnica de revelar ou ocultar inclui escolher quando exibir mais metadados. Com janelas menores, é melhor mostrar uma quantidade mínima de metadados. Em janelas maiores, uma quantidade significativa de metadados pode ser mostrada. Alguns exemplos de quando mostrar ou ocultar metadados incluem:

- Em um aplicativo de email, você pode exibir o avatar do usuário.
- Em um aplicativo de música, você pode exibir mais informações sobre um álbum ou artista.
- Em um aplicativo de vídeo, você pode exibir mais informações sobre um filme ou um programa, como mostrar detalhes do elenco e da equipe.
- Em qualquer aplicativo, você pode separar colunas e revelar mais detalhes.
- Em qualquer aplicativo, você pode pegar algo que esteja empilhado verticalmente e dispor horizontalmente. Ao passar do telefone ou phablet para dispositivos maiores, os itens de lista empilhados podem mudar para revelar as linhas de itens de lista e colunas de metadados.

## <a name="replace"></a>Substituir

Esta técnica permite que você alterne a interface do usuário para um ponto de interrupção específico. Neste exemplo, o painel de navegação e sua interface de usuário compacta, e transitória funciona bem em uma tela menor; porém, em uma tela maior, as guias podem ser uma melhor opção.

![Substituir elementos de design](images/rsp-design/rspd-replace.gif)

O controle [NavigationView](../controls-and-patterns/navigationview.md) dá suporte a essa técnica responsiva, permitindo que os usuários definam a posição do painel como superior ou esquerdo.

## <a name="re-architect"></a>Rearquitetura

Você pode recolher ou bifurcar a arquitetura do seu aplicativo para melhor direcionar dispositivos específicos. Neste exemplo, expandir a janela mostra todo o padrão mestre/de detalhes.

![um exemplo de rearquitetura de uma interface do usuário](images/rsp-design/rspd-rearchitect.gif)

## <a name="related-topics"></a>Tópicos relacionados

- [Tamanhos de tela e pontos de interrupção](screen-sizes-and-breakpoints-for-responsive-design.md)
- [Layouts dinâmicos com XAML](layouts-with-xaml.md)
- [Controles e padrões da UWP](../controls-and-patterns/index.md)
